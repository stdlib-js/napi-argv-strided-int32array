<!--

@license Apache-2.0

Copyright (c) 2022 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->

<!-- lint disable maximum-heading-length -->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# argv_strided_int32array

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Convert a Node-API value representing a strided array to a signed 32-bit integer array.

<!-- Section to include introductory text. Make sure to keep an empty line after the intro `section` element and another before the `/section` close. -->

<section class="intro">

</section>

<!-- /.intro -->

<!-- Package usage documentation. -->

<section class="installation">

## Installation

```bash
npm install @stdlib/napi-argv-strided-int32array
```

</section>

<section class="usage">

## Usage

```javascript
var headerDir = require( '@stdlib/napi-argv-strided-int32array' );
```

#### headerDir

Absolute file path for the directory containing header files for C APIs.

```javascript
var dir = headerDir;
// returns <string>
```

</section>

<!-- /.usage -->

<!-- Package usage notes. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="notes">

</section>

<!-- /.notes -->

<!-- Package usage examples. -->

<section class="examples">

## Examples

```javascript
var headerDir = require( '@stdlib/napi-argv-strided-int32array' );

console.log( headerDir );
// => <string>
```

</section>

<!-- /.examples -->

<!-- C interface documentation. -->

* * *

<section class="c">

## C APIs

<!-- Section to include introductory text. Make sure to keep an empty line after the intro `section` element and another before the `/section` close. -->

<section class="intro">

</section>

<!-- /.intro -->

<!-- C usage documentation. -->

<section class="usage">

### Usage

```c
#include "stdlib/napi/argv_strided_int32array.h"
```

#### stdlib_napi_argv_strided_int32array( env, N, stride, value, \*\*data, \*message1, \*message2, \*err )

Converts a Node-API value representing a strided array to a signed 32-bit integer array.

```c
#include "stdlib/napi/argv_strided_int32array.h"
#include <node_api.h>
#include <stdint.h>

static napi_value addon( napi_env env, napi_callback_info info ) {
    napi_value value;

    // ...

    int64_t N = 100;
    int64_t stride = 1;

    // ...

    int32_t *X;
    napi_value err;
    napi_status status = stdlib_napi_argv_strided_int32array( env, N, stride, value, &X, "Must be a typed array.", "Must have sufficient elements.", &err );
    assert( status == napi_ok );
    if ( err != NULL ) {
        assert( napi_throw( env, err ) == napi_ok );
        return NULL;
    }

    // ...
}
```

The function accepts the following arguments:

-   **env**: `[in] napi_env` environment under which the function is invoked.
-   **N**: `[in] int64_t` number of indexed elements.
-   **stride**: `[in] int64_t` stride length.
-   **value**: `[in] napi_value` Node-API value.
-   **data**: `[out] int32_t**` pointer for returning a reference to the output array.
-   **message1**: `[in] char*` error message if a value is not an `Int32Array`.
-   **message2**: `[in] char*` error message if a value has insufficient elements.
-   **err**: `[out] napi_value*` pointer for storing a JavaScript error. If not provided a number, the function sets `err` with a JavaScript error; otherwise, `err` is set to `NULL`.

```c
napi_status stdlib_napi_argv_strided_int32array( const napi_env env, const int64_t N, const int64_t stride, const napi_value value, int32_t **data, const char *message1, const char *message2, napi_value *err );
```

The function returns a `napi_status` status code indicating success or failure (returns `napi_ok` if success).

#### STDLIB_NAPI_ARGV_STRIDED_INT32ARRAY( env, X, N, stride, argv, index )

Macro for converting an add-on callback argument to a strided signed 32-bit integer array.

```c
#include "stdlib/napi/argv_strided_int32array.h"
#include "stdlib/napi_argv_int64.h"
#include "stdlib/napi/argv.h"
#include <node_api.h>
#include <stdint.h>

static void fcn( const int64_t N, const int32_t *X, const int64_t strideX, int32_t *Y, const int64_t strideY ) {
    int64_t i;
    for ( i = 0; i < N; i++ ) {
        Y[ i*strideY ] = X[ i*strideX ];
    }
}

// ...

static napi_value addon( napi_env env, napi_callback_info info ) {
    // Retrieve add-on callback arguments:
    STDLIB_NAPI_ARGV( env, info, argv, argc, 5 );

    // Convert the number of indexed elements to a C type:
    STDLIB_NAPI_ARGV_INT64( env, N, argv, 0 );

    // Convert the stride arguments to C types:
    STDLIB_NAPI_ARGV_INT64( env, strideX, argv, 2 );
    STDLIB_NAPI_ARGV_INT64( env, strideY, argv, 4 );

    // Convert the arrays a C types:
    STDLIB_NAPI_ARGV_STRIDED_INT32ARRAY( env, X, N, strideX, argv, 1 );
    STDLIB_NAPI_ARGV_STRIDED_INT32ARRAY( env, Y, N, strideY, argv, 3 );

    // ...

    fcn( N, X, strideX, Y, strideY );
}
```

The macro expects the following arguments:

-   **env**: environment under which the callback is invoked.
-   **X**: output variable name for the array.
-   **N**: number of indexed elements.
-   **stride**: stride length.
-   **argv**: name of the variable containing add-on callback arguments.
-   **index**: argument index.

</section>

<!-- /.usage -->

<!-- C API usage notes. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="notes">

</section>

<!-- /.notes -->

<!-- C API usage examples. -->

<section class="examples">

</section>

<!-- /.examples -->

</section>

<!-- /.c -->

<!-- Section to include cited references. If references are included, add a horizontal rule *before* the section. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="references">

</section>

<!-- /.references -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library for JavaScript and Node.js, with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2025. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/napi-argv-strided-int32array.svg
[npm-url]: https://npmjs.org/package/@stdlib/napi-argv-strided-int32array

[test-image]: https://github.com/stdlib-js/napi-argv-strided-int32array/actions/workflows/test.yml/badge.svg?branch=main
[test-url]: https://github.com/stdlib-js/napi-argv-strided-int32array/actions/workflows/test.yml?query=branch:main

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/napi-argv-strided-int32array/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/napi-argv-strided-int32array?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/napi-argv-strided-int32array.svg
[dependencies-url]: https://david-dm.org/stdlib-js/napi-argv-strided-int32array/main

-->

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://app.gitter.im/#/room/#stdlib-js_stdlib:gitter.im

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/napi-argv-strided-int32array/main/LICENSE

</section>

<!-- /.links -->
