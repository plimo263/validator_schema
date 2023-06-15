# Validator-Schema

Esta é uma lib usada para validar que vem de um jsonschema em API Rest. A capacidade de validação dele
vem de expressões regulares, strings, numeros, enums, datas (formato YYYY-MM-DD) e hora (formato HH:MM)

[MANUAL DA API ](https://plimo263.github.io/validator_schema/)

## INSTALAÇÃO

Para realizar a instalação deste módulo basta executar

```
pip install validator-schema
```

### UTILIZAÇÃO

Vamos supor que você tenha um servidor com Flask onde você vai receber por paramêtros um json e quer validar
os campos _name_ e _password_. Onde o _name_ não deve vir vazio e _password_ deve ter ao menos 8 caracteres.
Isto pode ser validado de forma simples com o validator-schema

```
from flask import request
import json
from validator_schema import ValidatorString, Validator

...

@app.route('/', methods = ['POST'])
def login():
    data = request.get_json()

    # An list of validators (required for Validator)
    list_validators = [
        ValidatorString('name', min = 1, msg_error = 'Field name without value'),
        ValidatorString('password', min = 8, msg_error = 'Field password minimum 8 caracters.')
    ]
    # An list of names requireds (required for Validator)
    requireds = ['name', 'password']

    # Object Validator for validation
    v = Validator(list_validators, requireds)

    try:
        v.is_valid(data)
    except ValueError as err: # One of fields not accepts
        return json.dumps({'error': str(err)})

    # Fields validates with success

```
