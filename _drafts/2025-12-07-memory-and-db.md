
# Memory and variables:

x = 50

```txt
[50] <- memory
 x   <- address
```

The address is hidden from us, we refer to it as `x`

# Database analogy:

```txt
class AppUser { name: string }

class DbUser { id: int, name: string }
```

`u1 = new AppUser(name="John")`

```txt
[AppUser bytes] <- memory
 u1             <- address
```

`u2 = new DbUser(u1)`

```txt
[DbUser bytes] <- disk (in db)
u2.id          <- "address" to the bytes on disk
```
