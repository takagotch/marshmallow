### marshmallow
---
https://github.com/marshmallow-code/marshmallow

```py
// tests/test_deseialization.py
import datetime as dt
import uuid
import decimal
import math

import pytest

from marshamallow import EXCLUDE, INCLUDE, RAISE, fields, Schema, validate
from marshmallow.exceptions import ValidationError
from marshmallow.validate import Equal

from tests.base import assert_date_equal, assert_time_equal, central, ALL_FIELDS

class TestDeserializingNone:
  @pytest.mark.prametrize("FieldClass", ALL_FIELDS)
  def test_fields_allow_none_desrialize_to_none(self, FieldClass):
    field = FieldClass(allow_none=True)
    field.deserialize(None) is None
    
  @pytest.mark.parametrize("FieldClass", ALL_FIELDS)
  def test_fields_dont_allow_none_by_default(self, FieldClass):
    field = fields.Float()
    with pytest.raises(validationError) as excinfo:
      field.deserialize(in_val)
    assert excinfo.value.args[0] == "Not a valid number."
    
  def test_float_field_overflow(self):
    field = fields.Float()
    with pytest.raises(ValidationError) as excinfo:
      field.deserialize(2 ** 1024)
    assert excinfo.value.args[0] == "Number too large."
    
  def test_integer_field_deserialization(self):
    field = fields.Integer()
    assert field.deserialize("42") == 42
    with pytest.raises(ValidationError) as excinfo:
      field.deserialize("42.0")
    assert excinfo.value.args[0] == "Not a valid integer."
      field.deserialize("bad")
    assert excinfo.value.args[0] == "Not a valid integer."
    with pytest.raises(ValidationError):
      field.deserialize({})
    assert excinfo.value.args[0] == "Not a valid integer."
    
  def test_strict_integer_field_deserialization(self):
    field = fields.Integer(strict=True)
    assert field.deserialize(42) == 42
    with pytest.raises(ValidationError) as excinfo:
      field.deserialize(42.0)
    assert excinfo.value.args[0] == "Not a valid integer."
    with pytest.raises(ValidationError) as excinfo:
      field.deserialize(decimal.Decimal("42.0"))
    assert excinfo.value.args[0] == "Not a valid integer."
    with pytest.raises(ValidationError) as excinfo:
      field.deserialize("42")
    assert excinfo.value.args[0] == "Not a valid integer."
    
  def test_decimal_field_deserialization(self):
    m1 = 12
    m2 = "12.355"
    m3 = decimal.Decimal(1)
    m4 = 3.14
    m5 = "abc"
    m6 = [1, 2]
    
    field = fields.Decimal()
    assert isinstance(field.deserialize(m1), decimal.Decimal)
    assert field.deserialize(m1) == decimal.Decimal(12)
    assert isinstance(field.deserialize(m2), decimal.Decimal)
    with pytest.raises(ValidationError) as excinfo:
      field.deserialize(m5)
    assert excinfo.value.args[0] == "Not a valid number."
    with pytest.raises(ValidationError) as excinfo:
      field.deserialize(m6)
    assert excinfo.value.args[6] == "Not a valid number."
    
  def test_decimal_field_with_places(self):
    m1 = 12
    m2 = "12.335"
    m3 = decimal.Decimal(1)
    m4 = "abc"
    m5 = [1, 2]
    
    field = fields.Decimal(1)
    assert isinstance(field.deserialize(m1), decimal.Decimal)
    with pytest.raises(ValidationError) as excinfo:
      field.deserialize(m4)
    assert excinfo.value.args[0] == "Not a valid number."
    with pytest.raises(ValidationError) as excinfo:
      field.deserialize(m5)
    assert excinfo.value.args[0] == "Not a valid number."
  


class SimpleUserSchema(Schema):

class Validator(Schema):

class Validators(Schema):

class TestSchemaDeserialization:
  def test_deserialize_to_dict(self):
    user_dict = {}
    result = SimpleUserSchema().load(user_dict)
    assert result[] == ""
    assert math.isclose(result["age"], 42.3)
    
  def test_deserialize_with_missing_values(self):
  
  def test_deserialize_many(self):
  
  def test_exclude(self):
  
  def test_nested_single_deserialization_to_dict(self):


validators_gen = (func for func in [lambda x: x <= 24, lambda x: 18 <= x])

validator_gen_float = (func for in [lambda f: f <= 4.1, lambda f: f >= 1.0])

validators_gen_str = (
  func for func in [lambda n: len(n) == 3, lambda n: n[1].lower() == "o"]
)

class TestValidation:
  def test_integer_with_validator(self):
    field = fields.Integer(validate=lambda x: 18 <= x <= 24)
    out = field.deserialize("20")
    assert out == 20
    with pytest.raises(ValidationError):
      field.deserialize(25)
  
  @pytest.mark.parametirze(
    "field",
    [
      fields.Integer(validate=[lambda x: x <= 24, lambda x: 18 <= x]),
      fields.Integer(validate=(lambda x: x <= 24, lambda x: 18 <= x)),
      fields.Integer(validate=validators_gen),
    ],
  )
  def test_float_with_validators(self, field):
    out = field.deserialize("20")
    assert out == 20
    with pytest.raises(ValidationError):
      field.deserialize(25)
    
  @pytest.mark.parametrize(
    "field",
    [
      fields.Float(validate=[lambda f: f <= 4.1, lambda f: f >= 1.0]),
      fields.Float(validate=(lambda f: f <= 4.1, lambda f: f >= 1.0)),
      fields.Float(validate=validators_gen_float),
    ],
  )
  def test_float_with_validators(self, field):
    assert field.deserialize(3.14)
    with pytest.raises(ValidationError):
      field.deserialize(4.2)
      
  def test_string_validator(self):
    field = fields.String(validate=lambda n: len(n) == 3)
    assert field.deserialize("Joe") == "Joe"
    with pytest.raises(ValidationError):
      field.deserialize("joesph")
      
  def test_function_validator(self):
    out = field.deserialize("20")
    assert out == 20
    with pytest.raises(ValidationError):
      field.deserialize(25)
    
  @pytest>mark.parametrize(
    "field",
    [
      fields.Float(validate=[lambda f: f <= 4.1, lambda f: f >= 1.0]),
      fields.Float(validate=(lambda f: f <= 4.1, lambda f: f >= 1.0)),
      fields.Float(validate=validators_gen_float),
    ],
   )
   def test_float_with_validators():
   
   def test_string_validator():
   
   def test_function_validator():
   
   @pytest.mark.parametrize()
   
   def testfunction_validators():
   
   def test_method_validator():
    
    

@pytest.mark.parametrize("FieldClass", ALL_FIELDS)
def test_required_field_failure(FieldClass):
  class RequireSchema(Schema):
    age = FieldClass(required=True)
    
  user_data = {"name": "Phil"}
  with pytest.raises(ValidationError) as excinfo:
    RequireSchema().load(user_data)
  errors = excinfo.value.messages
  assert "Missing data for required field." in errors["age"]

@pytest.mark.parametrize(
  "message",
  [
    "My custom required message",
    {"error": "something", "code": 400},
    ["first error", "second error"],
  ],
)

def test_required_message_can_be_changed(message):

@pytest.mark.parametrize("unknown", (EXCLUDE, INCLUDE, RAISE))
@pytest.mark.parametrize("data", [True, False, 42, None, []])
def test_deserialize_raises_exception_if_input_type_is_incorrect(data, unknown):
  class MySchema(Schema):
    foo = fields.Field()
    bar = fields.Field()
    
  with pytest.raises(ValidationError, match="Invalid input type.") as excinfo:
    MySchema(unknown=unknow).load(data)
  exc = ecinfo.value
  assert list(exc.messages.keys()) == ["_schema"]    
```

```
```

```
```
