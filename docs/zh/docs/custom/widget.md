# 组件

There are three kinds of widget which can be customized, and all of them are subclass of `Widget`.

```python
class Widget:
    templates: Jinja2Templates = t
    template: str = ""

    def __init__(self, **context):
        """
        All context will pass to template render if template is not empty.
        :param context:
        """
        self.context = context

    async def render(self, request: Request, value: Any):
        if value is None:
            value = ""
        if not self.template:
            return value
        return self.templates.get_template(self.template).render(value=value, **self.context)

```

## Display

If you want to custom display widget, just inherit `Display` and override the `render` method. Following example show
how to custom a display to show datetime.

```python
class DatetimeDisplay(Display):
    def __init__(self, format_: str = constants.DATETIME_FORMAT):
        super().__init__()
        self.format_ = format_

    async def render(self, request: Request, value: datetime):
        if isinstance(value, datetime):
            return await super(DatetimeDisplay, self).render(
                request, pendulum.instance(value).format(self.format_) if value else None
            )
        elif isinstance(value, date):
            return await super(DatetimeDisplay, self).render(
                request,
                pendulum.date(value.year, value.month, value.day).format(self.format_)
                if value
                else None,
            )
```

## Input

Different of `Display`, you should inherit `Input` and override `parse_value` and `render` methods. Following example
custom an input with html text input.

```python
class Text(Input):
    input_type: Optional[str] = "text"

    def __init__(
            self,
            help_text: Optional[str] = None,
            default: Any = None,
            null: bool = False,
            placeholder: str = "",
            disabled: bool = False,
    ):
        super().__init__(
            null=null,
            default=default,
            input_type=self.input_type,
            placeholder=placeholder,
            disabled=disabled,
            help_text=help_text,
        )
```

## Filter

For filter, just inherit `Filter`.

```python
class Search(Filter):
    template = "widgets/filters/search.html"

    def __init__(
            self,
            name: str,
            label: str,
            search_mode: str = "equal",
            placeholder: str = "",
            null: bool = True,
    ):
        """
        Search for keyword
        :param name:
        :param label:
        :param search_mode: equal,contains,icontains,startswith,istartswith,endswith,iendswith,iexact,search
        """
        if search_mode == "equal":
            super().__init__(name, label, placeholder, null)
        else:
            super().__init__(name + "__" + search_mode, label, placeholder)
        self.context.update(search_mode=search_mode)
```
