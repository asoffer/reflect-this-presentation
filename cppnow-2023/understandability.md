## Understandability

> "Using &nbsp;`Extend`&nbsp; with my class gives me incomprehensible error messages."
<!-- .element class="blockquote1" -->

NOTES:

The most common problems we saw were that people forgot the `friend EnableExtensions`.

@@@

## Understandability

```txt[|2-3|6-12|14]
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

Not every solution needs to be hi-tech.
