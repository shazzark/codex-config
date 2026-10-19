# Language Documentation Standards

Use the convention already established by the repository and the documentation toolchain. These links are starting points, not a requirement to adopt a new format or document every declaration.

## TypeScript and JavaScript

- For TypeScript API comments, follow the project's TSDoc conventions and supported tags. [TSDoc specification](https://tsdoc.org/pages/spec/overview/)
- For JavaScript, use JSDoc syntax when the project consumes it. If TypeScript checks JavaScript, confirm which JSDoc tags that TypeScript version supports; not every JSDoc tag has type-checking meaning. [JSDoc getting started](https://jsdoc.app/about-getting-started), [TypeScript JSDoc reference](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html)
- Keep documentation types and examples consistent with the actual declarations and supported runtime behavior.

## Python

- Follow the package's established docstring style. PEP 257 describes docstring conventions; it does not mandate a particular markup format such as Google or NumPy style.
- Document behavior, arguments, return values, side effects, exceptions, and restrictions when relevant to the callable's contract. [PEP 257](https://peps.python.org/pep-0257/)

## Go

- Follow Go doc-comment conventions for packages and exported declarations. Place comments where Go documentation tools associate them with the declaration and keep package comments useful to package readers. [Go Doc Comments](https://go.dev/doc/comment)

## Other languages

Follow the project's existing formatter, doc generator, and language or framework conventions. Check current authoritative documentation when exact syntax or tool behavior matters; do not transfer conventions from another language without confirming they apply.
