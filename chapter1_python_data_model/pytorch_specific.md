1. Operator overloading

```
__add__ => x + y
__mul__ => x*mask (elementwise mult)
__matmul__ => A @ B
__sub__ => A - B
__truediv__ => A/B (elementwise div)
```

Pytorch implementations of these internally takes care of: broadcasting, custom tensor wrappers etc.

2. Model calls

- forward pass
- hooks (Ex: can register register_forward_hook to nn layers) (pre and post forward hooks) : used to inspect/modify activations
[which is why dev shouldnt call model.forward(x) directly and do model(x) instead to utilize the data model implementation]
- manages autograd context

```
__call__()
```


3. Iterators

DataLoader is iterable

Datasets are iterable

Many ML abstractions rely on this

```
__iter__()
__next__()
```

4. Truthiness

```
Both NumPy and PyTorch forbid the direct boolean evaluation (e.g., if array:) of multi-element arrays/tensors, raising a ValueError or RuntimeError regarding "truth value ambiguity". Both frameworks 
require explicit reduction methods like .any() or .all() to condense multiple boolean values into a single scalar for conditional statements. 
PyTorch Forums
PyTorch Forums
 +2
Key Aspects of Ambiguous Evaluation:
The Issue: When comparing arrays (
), you get a multi-element boolean mask. Python cannot determine if the entire collection is True or False.
NumPy (ValueError): ValueError: The truth value of an array with more than one element is ambiguous. Use a.any() or a.all().
PyTorch (RuntimeError): RuntimeError: Boolean value of Tensor with more than one value is ambiguous.
Solutions:
.any(): Returns True if at least one element is True.
.all(): Returns True only if all elements are True.
.item(): Use only if the tensor/array has exactly one element.
Bitwise vs. Logical: Use & (and), | (or), ~ (not) for element-wise comparisons instead of Python's and, or, not, which fail on arrays.
```

5. Context Managers


```
with torch.no_grad():
```
```
Upon entering the with block, it disables gradient calculation for all operations inside that block.
Upon exiting the with block, it restores the previous autograd state, re-enabling gradient tracking if it was previously active.
```

6. Inheritance and MRO

```
class A:
    def hello(self):
        print("A")

class B:
    def hello(self):
        print("B")

class C(A, B):
    pass

c = C()
c.hello()

print(C.__mro__)
(<class '__main__.C'>,
 <class '__main__.A'>,
 <class '__main__.B'>,
 <class 'object'>)
```

```
nn.Module is not a standalone class.

It inherits from other classes.

When you call:

model(x)

Python checks:

Does MyModel define __call__?

If not → check nn.Module

If not → check its parent

Eventually → object

Same with:

__setattr__

__getattr__

__repr__

Inheritance + MRO decide everything.
```

```
When you write:

super().__setattr__(name, value)

Python doesn’t just go “to parent.”

It follows the MRO order.

That’s very important in multiple inheritance.
```

practical example
```
import torch.nn as nn

class MyModel(nn.Module):
    def forward(self, x):
        return x

When you do:

model(x)

Python:

Finds no __call__ in MyModel

Looks in nn.Module

Uses its __call__

That calls your forward

That is inheritance + MRO in action.
```




