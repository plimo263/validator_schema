# Bem-vindo ao validator_schema

Esta é uma lib com recursos para validação de campos do jsonschema que pode ser
usado em APIRest.

## Exemplo de uso

Vamos supor que você tenha um servidor com Flask onde você vai receber por paramêtros um json e quer validar
os campos _name_ e _password_. Onde o _name_ não deve vir vazio e _password_ deve ter ao menos 8 caracteres.
Isto pode ser validado de forma simples com o validator-schema

```{.py3 title='Exemplo de uso'}
from flask import request
import json
from validator_schema import ValidatorString, Validator

...

@app.route('/', methods = ['POST'])
def login():
    data = request.get_json()

    # Uma lista de validators (requerido por Validator)
    list_validators = [
        ValidatorString(
            'name', min = 1,
            msg_error = 'Field name without value'
        ),
        ValidatorString(
            'password', min = 8,
            msg_error = 'Field password minimum 8 caracters.'
        )
    ]
    # Uma lista de nomes requeridos (também necessário para Validator)
    requireds = ['name', 'password']

    # Uma instancia do objeto Validator
    v = Validator(list_validators, requireds)

    try:
        v.is_valid(data)
    except ValueError as err: # Quando um ou mais campos não são validos
        return json.dumps({'error': str(err)})

    # Todos os campos passaram no teste.

```

Este é um dos dos casos de uso, para mais detalhes veja os manuais relacionados as classes.
