# Excel revision problem in pandas

When using *input()* function in terminal, input value will be written into memory as string format.

eg.

```python
rev = input()
print(type(rev))
```

No matter what you type, the output will be 

```python
<class 'str'>
```

However, in input_files.xlsx, some parameters are string while other are numbers, including int and float. The *read_excel* and *to_excel* function will not change the type of variables. A float read in memory will be a float and a string in memory can only be written in to excel files as a string.

Some blogs mentioned that *to_excel* function includes a parameter named 'float_format', which can be set `pd.to_excel(path, float_format = '%.4f')`. But it will only function on float variables in the dataframe instead of revising a string shown like a float even. For example, '32.767'. When the excel is read into memory again. It is '32.767' instead of float 32.767. 

Hence, in order to keep the original type in and out of memory. Following codes are used.

```python
print("You want to change %s into " % str(parameters.loc[var_index, 'variables']))
print("Please be notified, your change will modify the default parameter file")
rev = input()
try:
    rev_num = float(rev)
except:
    rev_num = rev
finally:
    parameters.loc[var_index, 'input'] = rev_num
parameters.to_excel('input_files.xlsx', index = False)
```

If it is a number, it will be transformed into a float in `try`. If not, it will remain as a string in `except`.
