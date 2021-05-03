# pyinstaller summary

## 1. generate spec files

activate your virtual env in cmd, input

```python
pyinstaller main.py
```

Then a *build* and a *dist* file folder will generate but can be deleted.

And a spec file is generated too. Spec file is a Python script.

```python
# -*- mode: python ; coding: utf-8 -*-

block_cipher = None


a = Analysis(['main.py'],
             pathex=['test.py', 'C:\\Users\\ZZN4SGH\\projects\\soot_loading\\deployment'],
             binaries=[],
             datas=[('input_files.xlsx', '.'), ('GUROBI_RUN.py' , '.')],
             hiddenimports=[],
             hookspath=[],
             runtime_hooks=[],
             excludes=[],
             win_no_prefer_redirects=False,
             win_private_assemblies=False,
             cipher=block_cipher,
             noarchive=False)
pyz = PYZ(a.pure, a.zipped_data,
             cipher=block_cipher)
exe = EXE(pyz,
          a.scripts,
          [],
          exclude_binaries=True,
          name='main',
          debug=False,
          bootloader_ignore_signals=False,
          strip=False,
          upx=True,
          console=True )
coll = COLLECT(exe,
               a.binaries,
               a.zipfiles,
               a.datas,
               strip=False,
               upx=True,
               upx_exclude=[],
               name='main')


```

You can either package your program as a whole exe or file folder. It depends on your initial packaging command or your spec file. Above is the file folder situation.



## 2. Revise your spec file.

1. put your main function in the first variable of *a*. The exe will enter this python file.

2. put your other py files and their path in *pathx*. Be noticed that if you have imported all required manual py modules in main.py, you do not have to add those py files here and the pyinstaller will package them automatically.

3. put your data files in *datas*. The data files will not be encrypted and will just be put in the file folder as original. In other words, if they are not added in the spec files, they can be put in the packaged file folder *dist/main* immediately. They are equal.



## 3. Rerun pyinstaller in cmd

```python
pyinstaller main.spec
```

1. When packaging, some warning will print in the cmd or in the *build/main/warning.txt*, You can add those modules in *hidden_import*. It is suggested to copy module parameters from similar spec files instead of adding them on your own, which would cost a lot of time.

2. Other problems will appear, such as Soot_engine project, There always exists a problem that *No Module Named 'GUROBI_RUN'*. I have made efforts on every solution on the Internet including adding *pathx*, *hidenimports*, and write ```from *** import GORUBI_RUN```in main.py, but all failed. I tried to put the file *GUROBI_RUN.py* in the *dist/main* file folder immediately. Miraclously, it works!



## 4. check your main.exe

If your main.exe goes wrong, it will exit promptly. Therefore it is suggested that run the exe in cmd windows. Then if it exits, the cmd windows will not close and you can see the print.





Reference:

[Pyinstaller 打包发布经验总结_YanHua_jake的博客-CSDN博客_pyinstaller](https://blog.csdn.net/weixin_42052836/article/details/82315118)


