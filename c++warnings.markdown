---
title: ALICE SW Development best practices
subtitle: C++ Warnings Troubleshooting
layout: main
---

This is an incomplete list of warnings which have happened in AliPhysics
and how to clean them up.

### 'delete' applied to a pointer that was allocated with 'new[]'; did you mean 'delete[]'?

Not everyone remembers that in C++ there is a special operator to delete arrays
allocated with new, `delete[]`.

This means that if you have allocated a vector with something similar to:

    Int_t* a = new Int_t[N];

you need to delete it with:

    delete[] a;

See also:

- <http://en.cppreference.com/w/cpp/memory/new/operator_new>
- <http://en.cppreference.com/w/cpp/memory/new/operator_delete>
