## Boost.pfr

```
struct UserInfo {
    int64_t id;
    std::string name, email, login;
};
```

| Goal              | Rating |
| ----------------- | ------ |
| Evolution         |     D- |
| Understandability |     A  |
| Development Speed |     A+ |

NOTES:

D- because you can add fields easily, but you are forced to make everything
public. Maybe D- is to harsh here. And users get everything/anything they might
ever want.

Clarify long-term evolution isn't the goal of Boost.pfr. It's primary focus is
dev speed.

@@@

## TUPLE_DEFINE_STRUCT
```
struct UserInfo {
    TUPLE_DEFINE_STRUCT(UserInfo, (eq, ne, stream),
    (int64_t, id),
    (std::string, name),
    (std::string, email),
    (std::string, login));
};
```

| Goal              | Rating |
| ----------------- | ------ |
| Evolution         |     B+ |
| Understandability |     C- |
| Development Speed |     B+ |


@@@

## Extend
```
struct UserInfo : Extend<UserInfo>::With<EqualityExtension
                                         DebugPrintingExtension> {
    int64_t id;
    std::string name;
    std::string email;
    std::string login;
};
```

| Goal              | Rating |
| ----------------- | ------ |
| Evolution         |     A  |
| Understandability |     A- |
| Development Speed |     B+ |

@@@

## Odin Holmes
https://www.youtube.com/watch?v=VMOsz61Hg84