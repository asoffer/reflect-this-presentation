## Easy To Use
* Dependencies.
@@@

## Hard to misuse

@@@

```cc[]
class SafeString : public Extend<SafeString, 2>::With<EqualityExtesnion> {
   explicit SafeString(std::string s) : s_(std::move(S)) {}

   std::string copy() cons;

  private:
   friend EnableExtensions;

   std::mutex m_;
   std::string s_;
};
```

NOTES:

With a naive implementation, this will compile-just fine up until someone
actually goes to use equality. We added hooks so users could instantiate
dependent expressions in the types constructor. Not perfect, but will be
surfaced earlier (when the type is constructed, even if the extension isn't
used.

Credit: Daisy Hollman.

@@@

## Error messages

```txt
extend.h:xx:yy: error: static assertion failed due to requirement 'kFailedToCountFields': Could not detect the number of fields. If you're using a class with private fields, you must specify the number of fields as a second parameter template to `Extend`. For example:
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


@@@

* static_assert return tuple()? Might not be worth mentioning.

@@@

DebugPrinting.
