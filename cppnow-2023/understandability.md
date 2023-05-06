## Understandability

> "Using &nbsp;`Extend`&nbsp; with my class gives me useless error messages."
<!-- .element class="blockquote2" -->

NOTES:

The most common problems we saw were that people forgot the `friend EnableExtensions`.

@@@

## Understandability

```txt[|2-4|6-12|14]
extend.h:xx:yy: error: static assertion failed due to requirement
'kFailedToCountFields': Could not detect the number of fields. If you're using a
class with private fields, you must specify the number of fields as a second
parameter template to `Extend`. For example:
    ```
    class MyType : public Extend<MyType, FIELD_COUNT_GOES_HERE>::With<...> {
     public:
      ...
     private:
      friend EnableExtensions;  // Don't forget this too!
     ...
    };
    ```
See ABSEIL_TIP#extending-classes for a worked example.
```
<!-- .element style="font-size:12pt;" -->

NOTES:

* Not every solution needs to be hi-tech.

* Having someone personally thank me for writing a `static_assert` is an item I
  did not know was on my bucket list.
