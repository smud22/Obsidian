Discards leading whitespace from an input stream.

Behaves as an [UnformattedInputFunction](https://en.cppreference.com/w/cpp/named_req/UnformattedInputFunction.html "cpp/named req/UnformattedInputFunction"), except that is.gcount() is not modified. After constructing and checking the sentry object, extracts characters from the stream and discards them until any one of the following conditions occurs:

- end of file condition occurs in the input sequence (in which case the function calls setstate(eofbit) but does not set `failbit`; this does not apply if the `eofbit` is already set on is prior to the call to `ws`, in which case the construction of the sentry object would set `failbit`).

- the next available character c in the input sequence is not whitespace as determined by [std::isspace](https://en.cppreference.com/w/cpp/string/byte/isspace.html)(c, is.getloc()). The non-whitespace character is not extracted.

This is an input-only I/O manipulator, it may be called with an expression such as in >> std::ws for any in of type [std::basic_istream](https://en.cppreference.com/w/cpp/io/basic_istream.html "cpp/io/basic istream").

### Parameters

|   |   |   |
|---|---|---|
|is|-|reference to input stream|

### Return value

is (reference to the stream after extraction of consecutive whitespace).

### Notes

If `eofbit` is set on the stream prior to the call, the construction of the sentry object will set `failbit`.

### Example

![[Pasted image 20251221163005.png]]