---
title: ALICE SW Development best practices
subtitle: C++ Warnings Troubleshooting
layout: main
---

This is an incomplete list of warnings which have happened in AliPhysics
and how to clean them up.

### ['delete' applied to a pointer that was allocated with 'new[]'; did you mean 'delete[]'?](#mismatched-new-delete)

Not everyone remembers that in C++ there is a special operator to delete arrays
allocated with new, `delete[]`.

This means that if you have allocated a vector with something similar to:

    Int_t* a = new Int_t[N];

you need to delete it with:

    delete[] a;

Otherwise this will result in a memory leak.

While by default this practice is considered a warning by the compiler,
due to the fact the standard actually allows it, this is never the
correct thing to do. For this reason, starting from 23/5/2016 AliPhysics
will treat this as an error.

See also:

- <http://en.cppreference.com/w/cpp/memory/new/operator_new>
- <http://en.cppreference.com/w/cpp/memory/new/operator_delete>

### [Delete called on 'BaseClass' that has virtual functions but non-virtual destructor `[-Werror,-Wdelete-non-virtual-dtor]`](#delete-non-virtual-dtor)

Whenever a C++ class has virtual methods and it's therefore meant to
be used polymorphically, it's good practice to make sure to provide a
virtual destructor:

```c++
class BaseClass {
public:
  virtual ~BaseClass() { ... }
};
```

This is because otherwise you will encounter an undefined behavior
whenever you try to delete a derived class through a pointer to the
baseclass. This also applies to any class which uses the ROOT
`ClassDef()` macro as it expands by adding virtual methods.

See also:

- <http://www.gotw.ca/publications/mill18.htm>
