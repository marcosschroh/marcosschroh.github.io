<!--
.. title: Let's help version controls
.. slug: lets-help-version-controls
.. date: 2019-04-14 17:01:16 UTC+02:00
.. tags: python
.. category: python, control version
.. link: 
.. description: 
.. type: text
-->

As developers sometimes we don't realize the amount of work that a control version has to do in order
to calculate diffs, so what can we do about it in order to help them and make our teammate's life easier? 

## A control version is ...

According to Wikipedia is the management of changes to documents, computer programs, large web sites, and other collections of information. Somehow, these guys have to track and compare every change that we made, crazy eh!

## Complacent comma placement

Let's imagine that we have the following python list and we are using `git` as our control version tool:

```python
my_list = ["banana", "orange", "apple"]
```

Whenever you make a change to the list, it will be hard to tell what was modified by looking
at git diff. Because of most of the source control system are line-based they have a hard time highlining multiple changes to a single line

A quick fix is to adopt a code style where you spread out list, dict or set constants across multiple lines,
so we have one item per line:

```python
my_list = [
    "banana",
    "orange",
    "apple"
]
```

Now, is perfectly clear when one item was added, removed or modified using a `git diff`.

As a bonus, let's say that you add `anana` at the end of the list, you commit but you can not push because you are not
in the latest version. Make a pull and there is a conflict because your teammate has added `pear` at the end of the list:

```python
my_list = [
    "banana",
    "orange",
    "apple",
    "anana" # <-- missing comma
    "pear"
]
```

Seems really clear here that you have to add a comma after `anana` to avoid `string literal concatenation`,
but it happened to me a lot of times that I was focused on trying to solve the `merge conflict` and I forgot the COMMA!!
Just for a missing comma and we have unexpected results.

So, as `python` developer we can adopt a code style that we place a comma after every item in a `list`, `dict` or `set constants` including the last item to avoid silly mistakes in merge conflicts.

```python
my_list = [
    "banana",
    "orange",
    "apple",
    "anana",
    "pear",
]
```

Unfortunately, not always we can do this, for example in `json`:

```json
{
    "firstName": "Foo",
    "lastName": "Bar", // <--  SyntaxError: Unexpected token } in JSON at position ...
}
```

## Space at the end

Why are line breaks important?

Version control systems are focused on text files; they can track the changes, merge files automatically, or facilitate the process of resolving conflicts. Because of this, line endings are crucial in understanding the content of the file and how to work with the changes (most of them do merges on a line by line basis).

For example, `Git` supports both `CR+LF` and `LF` line endings using several configuration options.

Git's default merge strategy will throw a conflict whenever two branches make changes to adjacent (or the same) lines. This is eminently sensible: when changes are made, neighboring lines are needed to give them context – simply merging changes when their context has also changed won't always give the desired result.

Let's imagine that we have a file called `foo.txt` and we add the word `python` without an `ending line`:

If run `git diff` we have the following message:

```bash
diff --git a/marcosschroh/foo.txt b/marcosschroh/foo.txt
index e69de29..d8654aa 100644
--- a/marcosschroh/foo.txt
+++ b/marcosschroh/foo.txt
@@ -0,0 +1 @@
+python
\ No newline at end of file
```

So far so good, we decide to commit and push. After a while, is time to add a new word to our foo file and we write `test` with a new line at the end. What do you think we will get after running `git diff`?

```bash
diff --git a/marcosschroh/foo.txt b/marcosschroh/foo.txt
index d8654aa..4215a2c 100644
--- a/marcosschroh/foo.txt
+++ b/marcosschroh/foo.txt
@@ -1 +1,2 @@
-python
\ No newline at end of file
+python
+test
```

but why? If I just added the word `test` and not `python`? Don't panic, the first line shows up in the diff as modified although there is no visible change because newlines are a control character and therefore don't have a visible representation.

Now, what would have the output of `git diff` if we had added a `new line` at the end of `python` and after we add the `test` word?

```bash
diff --git a/marcosschroh/foo.txt b/marcosschroh/foo.txt
index fdc793e..4215a2c 100644
--- a/marcosschroh/foo.txt
+++ b/marcosschroh/foo.txt
@@ -1 +1,2 @@
 python
+test
```

Cleaner and easier, right?


## Conslusion:

Adapting smart formattings like spread out list, dict or set constants across multiple lines and adding a comma at the end of
each item including the last one is easier to maintain and avoid bugs.
Always add a `new line` at the end, it will make life easier to your teammates and for version control tools.
