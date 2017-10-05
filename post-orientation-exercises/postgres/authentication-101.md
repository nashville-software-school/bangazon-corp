# Authentication

Authentication is the process of validating user credentials, such as a password. There are [three factors](https://en.wikipedia.org/wiki/Authentication#Factors_and_identity) of authentication, but this resource will focus on the **knowledge factor**. Examples of some elements belonging to the knowledge factor would be:
> Something the user knows (e.g., a password, Partial Password, pass phrase, or   personal identification number (PIN), challenge response (the user must answer a question, or pattern), Security question

Authenticating with only one element from one of the three factors would be referred to as single factor authentication. This is the most basic form of authentication, and **should not be used for personally sensitive transactions.**

Authentication should not be confused with **authorization**, which is the act of granting or rejecting a user access to resources based on specific rules or a policy.


## Authentication Best Practices

1. Never store plain text passwords
1. Make usernames case insensitive (e.g.: 'smith' is the same as 'Smith') as well as unique
1. For apps using personal information, **do not write your own authentication.** Using a third party service or library is a possible alternative if authentication is necessary.


## Encryption

Encryption is the process of using an algorithm to encode data (referred to as plain text) into unreadable data (referred to as cipher text) of undetermined length. An algorithm usually uses a self generated, pseudo-random encryption key to transform the data. Depending on the type of encryption, it could be possible to both encrypt and decrypt data with the same key.

#### Why use encryption?

Encryption is ideal for situations where data needs to be made secure, but at some point it will then need to be decrypted, such as secure messaging. This ability to reverse the encrypted data makes it less than ideal for authenticating a user's password. If the private encryption key is exposed or the algorithm that was used to encrypt the password is known, the password can possibly be decrypted.


## Hashing

An alternative to encryption would be to use a hash function. A hash function is a mathematical algorithm that can take data of any size (referred to as a message) and convert it into a string of a fixed size (referred to as a hash). Once the data has been hashed, there is no reversal process, unlike encryption. The only way to generate matching hashes is to have matching input messages. Even a small change to the message will generate an extensively different hash.

MD5 is the most widely known hashing function. It produces a 32 digit hexadecimal number for it's hash. Try running this command in the terminal to hash a string with MD5:

`echo 'hello' | md5  // OUTPUT=> b1946ac92492d2347c6235b4d2611184`

Now try running the same command. Notice how the hash does not change. Try hashing different strings, or change a single letter in `'hello'` and notice the difference in the hashes.

A common practice is to run the message through the hash function multiple times, which adds more randomness to the generated hash. Try running the following command and compare the hash to the first one:

`echo 'hello' | md5 | md5 // OUTPUT=> 5db9f475f4cb835f1afccf4bb1a706b7`

This ensures that even matching messages will produce different hashes if one is run through the hash function multiple times.


#### Salting a hash

There are weaknesses to storing hashes, such as a brute force attack where a computer would continually calculate hashes until it finds a match, as well as a [rainbow table attack](https://en.wikipedia.org/wiki/Rainbow_table). A rainbow table is a precomputed table of hashes that a computer can use to simply compare hashes instead of continually calculating hashes with a hash function.

A way to defend against these precomputed lists are to use [salts](https://en.wikipedia.org/wiki/Salt_(cryptography)), or randomly generated data that can be various lengths. A salt can be appended or prepended with a plaintext password and then hashed to create a new value which can then be stored. Salts are usually unique to an application or service, not per user.

```
// A hash with no salt
echo 'hello' | md5  // OUTPUT=> b1946ac92492d2347c6235b4d2611184

// A salted hash
echo 'hello' + 'QxLUF1bgIAdeQX' | md5  // OUTPUT=> 5a55d9024d7f065a08c69bc0ca35cbf2
```

#### Peppering a hash

For additional security, it is also common to pepper a salted and hashed password. Much like salts, a pepper is a randomly generated string. However, unlike salts, a pepper is unique to each value that will be hashed. A pepper is generated from a limited set of values, where each value is appended to the given data to be tested against the stored hash.

Check out [pepper](https://en.wikipedia.org/wiki/Pepper_(cryptography)) for more information.


### Additional Resources

- [Wikipedia Authentication](https://en.wikipedia.org/wiki/Authentication)
- [Cryptographic Hash Function](https://en.wikipedia.org/wiki/Cryptographic_hash_function)
- [Salt vs Pepper](http://simplicable.com/new/salt-vs-pepper)
