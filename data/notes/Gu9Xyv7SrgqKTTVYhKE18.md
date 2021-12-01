
# blueprint

Ads ranking model development can be represented by an execution graph. Blueprint is a solution to describe and execute such an execution graph.

![](/assets/images/2021-06-25-08-02-43.png)

![](/assets/images/2021-08-18-13-35-10.png)

![](/assets/images/2021-08-18-13-35-55.png)

![](/assets/images/2021-08-18-13-38-43.png)

goal

![](/assets/images/2021-08-18-13-40-31.png)


## IR

Execution graph is represented by a data structure which we call “Intermediate Representation” or IR. IR is in JSON format.

## Module

When implementing the function execute in GMBModule, we write normal Python code, which describes what execute does.

```
class GMBModule(BaseModule):
    @gmb_executor_method()
    def execute(params=...):
        process(params)
```

- When execution_mode = IR_GENERATION: calling execute(params=...) generates an IR which encodes the fact that the function execute will be called with params remotely later.
- When execution_mode = IR_EXECUTION: calling execute(params=...) executes process(params)

### edge between module

```
module_a = GMBModule()
module_b = GMBModule2()
module_a.execute()
module_b.set_params(params=module_a.get_result())
module_b.execute()
result = module_b.get_result()
```
## core

![](/assets/images/2021-08-18-13-53-29.png)

## BPS

![](/assets/images/2021-08-18-13-52-15.png)


![](/assets/images/2021-08-18-13-57-03.png)

## AIR

![](/assets/images/2021-08-18-14-02-44.png)