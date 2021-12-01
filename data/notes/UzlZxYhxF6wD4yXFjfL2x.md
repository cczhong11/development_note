
# alfred workflow


- Triggers: Activate Alfred from a hotkey, another Alfred feature or an external source.
- Inputs: Keyword-based objects used to perform an action, on its own or followed by a query.
- Actions: The objects that do most of the work in your workflows; opening or revealing files and web searches, running scripts and performing commands.
- Utilities: Utilities give you control over how your objects are connected together and how the arguments output by the previous object is passed on to the next object.
- Outputs: Collect the information from the earlier objects in your workflow to pop up a Notification Centre message, show output in Large Type, copy to clipboard or run a script containing the result of your workflow.


## script filter format in workflow

we need to return a json like this

```
{
    "items":[
        {
            "title":xxx,
            "subtitle":xxx,
            "arg":"https://xxx"
            "icon":"xxx"
        }
    ]
}
```

## short key

- option+command+c clipboard history