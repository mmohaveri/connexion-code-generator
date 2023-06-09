# connexion-code-generator

`connexion-code-generator` is a code CLI tool that generates Abstract or Protocol class definitions
based on your openapi.yaml specification. These class definitions then will be used to make sure our
connexion MethodViews are compatible with our API definition using static type checkers like `mypy` or `pyright`.

It currently only supports [**OpenAPI 3.0**](https://swagger.io/specification/v3/).

[![PyPI - Version](https://img.shields.io/pypi/v/connexion-code-generator.svg)](https://pypi.org/project/connexion-code-generator)
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/connexion-code-generator.svg)](https://pypi.org/project/connexion-code-generator)

-----

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [License](#license)

## Installation

`connexion-code-generator` is available on PyPi, so you can install it using the following command.

```console
pip install connexion-code-generator
```

Keep in mind that this project in beta stage so please pin the minor version of the package,
as there will be no backward incompatible changes in patch versions with the same minor version,
but there's no backward compatibility and API stability guaranteed between minor versions.

## Usage

To run it with its default config simply run the `connexion-protocol-generator generate` command
in the same directory as your `openapi.yaml` file. It will create an `api` python module with a
`view.py` file inside it.

### Using [datamodel-code-generator](https://github.com/koxudaxi/datamodel-code-generator) to generate DTOs

`connexion-code-generator` does not care about the way you convert your OpenAPI object schemas to python
classes, it just assumes that any object defined in your schema has an equivalent python class in the
`api.dtos` module. As the convention for schema names in OpenAPI are snake-case but the convention for
class names in python is PascalCase, it assumes the conversion has been done, i.g: an schema with the name
`some-api-request` should have a class named `SomeApiRequest`.

This is done to prevent re-inventing the wheel as there are many good library and CLI applications that
achieve this conversion task with a high quality.

We use `datamodel-code-generator` for this. We first run `datamodel-code-generator` on our schema and store its
result in the `api.dtos` module, then we run `connexion-code-generator`.

### Advance options

`connexion-code-generator` provides the following options (available to you by running `connexion-code-generator generate -h`):

- -i, --input **OpenAPI specification file, defaults to `./openapi.yaml`**
- -d, --dto-module-path **DTOs module name, defaults to `api.dtos`**
- -o, --output **Output file path, default to `./api/views.py`**
- -e, --error-type **The DTO type for unknown error responses, defaults to `ErrorResponse`**
- --abstract **Generate abstract class instead of protocol class**
- --sync **Generate sync methods instead of async methods**

## License

`connexion-code-generator` is distributed under the terms of the [MIT](https://spdx.org/licenses/MIT.html) license.
