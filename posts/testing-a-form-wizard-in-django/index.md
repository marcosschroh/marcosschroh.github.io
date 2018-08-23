<!--
.. title: Testing a Form Wizard in Django
.. slug: testing-a-form-wizard-in-django
.. date: 2017-11-30 10:08:43 UTC+01:00
.. tags: python
.. category: python, django, test, wizard
.. link: 
.. description: 
.. type: text
-->

I was writing a couple to tests form my django views and I got stuck with a Form Wizard. I tought.. well I will need to iterate over a form list and make several post as many steps as the wizard has. That assumetion was correct, so I created the data for every form, I create a form loop... and bump, It didn't work.

```shell
return handler(request, *args, **kwargs)
File "/home/marcos/.virtualenvs/platformv3/local/lib/python2.7/site-packages/formtools/wizard/views.py", line 284, in post code='missing_management_form',
ValidationError: [u'ManagementForm data is missing or has been tampered.']
```

&nbsp;

It didn't work because like we can see in the Traceback a wizard also need management_form.
Basically, I was makeing a post with the data for each form but of course teh Wizard needs to know wich step are you refering to.
So, let's go to the real example.

Let's imagine that these are my Forms.

```python
class TicketInfoForm(forms.Form):
    limit = forms.IntegerField(min_value=1)
    name = forms.CharField()
    pub_date = forms.DateTimeField()

class AddressForm(forms.Form):
    street_name = forms.CharField()
	zipcode = forms.CharField()
	city = forms.CharField()
	country = forms.CharField()
```

&nbsp;

My views.py file

```python
from django.shortcuts import redirect

from formtools.wizard.views import SessionWizardView

from .forms import TicketInfoForm, AddressForm

TICKETS_INFO_WIZARD_FORMS = (
    ("Ticket Information", TicketInfoForm),
    ("Address", AddressForm)
)

class TicketWizardFormView(SessionWizardView):
    template_name = 'tickets_info.html'

    def done(self, form_list, **kwargs):
        ticket_info_form, address_form = form_list

		address = address_form.save(commit=False)
        address.type = BUYER_ADDRESS
        address.save()

		tickets_info = tickets_info_form.save(commit=False)
        tickets_info.user = self.request.user
        tickets_info.address = address
        tickets_info.save()        

        return redirect(reverse('tickets_info_list'))
```

&nbsp;

My urls.py file


```python
from django.conf.urls import url, patterns
from django.contrib.auth.decorators import login_required

from .views import TicketsWizardFormView, TICKETS_INFO_WIZARD_FORMS

urlpatterns = patterns(
    ...
    url(r'^tickets-info/request/$', login_required(TicketsWizardFormView.as_view(TICKETS_INFO_WIZARD_FORMS, name='tickets_info_request'),
)
```

This code is working fine, with to test it we need to add and change some things.

1. Every time that we make a POST besides of send the data that each 
   form, we also need to send the current step: [wizard_name]-current_step: [current_step]
2. We need to change the fields name to: [step_name]-[field_name]: [value]

For example for our TicketsInfo Form:

```python
data_tickets_info_form = {
	'Ticket Information-limit': 10,
	'Ticket Information-name': 'My first Pool',
	'Ticket Information-pub_data': '2017-11-30T12:00:00',
	'ticket_wizard_form_view-current_step': 'Ticket Information'
}

```

&nbsp;

In the above example we can see that we have added the [current_step] value and each field has the [step_name] prefix.


Now we are ready to pass our test:

```python
# test_views.py

from django.test import TestCase

from django.core.urlresolvers import reverse

from django.contrib.auth.models import User


class TestViews(TestCase):

	fixtures = ['users.json']

	def setUp(self):
        self.client = Client()
        self.user = User.objects.first()

	def test_ticket_wizard_form(self):
        name = 'Awesome Tickets'
        limit = 10

        data_pool_form = {
			'Ticket Information-limit': limit,
			'Ticket Information-name': name,
			'Ticket Information-pub_data': '2020-11-30T12:00:00',
			'ticket_wizard_form_view-current_step': 'Ticket Information'
		}

		data_address_form = {
		 	'Address-street_name': 'Isac Newton',
			'Address-zipcode': '2011NA',
			'Address-city': 'Pythonic Straat',
			'Address-country': 'Netherlands',
			'ticket_wizard_form_view-current_step': 'Address'
		}

        TICKETS_STEPS_DATA = [data_pool_form, data_address_form]

        for step, data_step in enumerate(TICKETS_STEPS_DATA, 1):
            response = self.client.post(reverse('tickets_info_request'), data_step)

            if step == len(TICKETS_STEPS_DATA):
                # make sure that after the create pool we are redirected to Pool List Page
                self.assertRedirects(response, reverse('tickets_info_list'))
            else:
                self.assertEqual(response.status_code, 200)

        # get the pool
        TicketInfo.objects.get(
            name=name,
            limit=limit,
            user=self.user
        )

```

&nbsp;

And... The test pass :-)

```shell
....................................
----------------------------------------------------------------------
Ran 36 tests in 5.555s

OK
```

##Conclusion:

Test a Form Wizard is not so hard as seems at the beginning!!

