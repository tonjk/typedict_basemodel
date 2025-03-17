# typedict_basemodel
The difference between TypedDict and BaseModel


**TypedDict vs. BaseModel**

**1. Origin and Purpose:**

  * `TypedDict` comes from Python's typing module and is primarily for type annotations.
  * `BaseModel` comes from Pydantic and is for data validation, parsing, and serialization.

**2. Runtime Behavior:**

  * `TypedDict` provides only type hints for static type checkers like mypy. It has no runtime effect.
  * `BaseModel` actually validates data at runtime, converts types, and generates helpful error messages.

**3. Usage Pattern:**

  * With `TypedDict`, you're just creating a dictionary with type hints:
  ```python
  my_dict: format_typedict = {"series": "Series1", "color": "Red"}
  ```
  
  * With `BaseModel`, you're creating an object instance:
  ```python
  my_model = format_basemodel(series="Series1", color="Red")
  ```

**4. Validation:**

* `TypedDict` doesn't validate anything at runtime.
* `BaseModel` validates data when an instance is created and when values are set.

#### Your Specific Examples
In your first example:
```python
class format_response(TypedDict):
    series: Optional[Literal[tuple(series)]]
    color: Optional[Literal[tuple(color_unique)]]
    pgroup: Optional[Literal[tuple(group_unique)]]
```

  * This is attempting to use *Literal* types with dynamically created tuples which is causing your error
  * This would only provide type hints without runtime validation
  * *Optional* fields in *TypedDict* are annotated but still required without using *total=False*

In your second example:
```python
class format_basemodel(BaseModel):
    series: Optional[str] = Field(None, description="Series of product")
    color: Optional[str] = Field(None, description="color of product")
    pgroup: Optional[str] = Field(None, description="group name of product")
```

 * This provides full runtime validation
 * The *Field(None, ...)* syntax makes these fields truly optional
 * The validation is against any string, not restricted to specific values
 * Includes documentation via *description*
