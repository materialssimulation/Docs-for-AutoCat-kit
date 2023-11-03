**吸附构型的高通量建模**
========================================

（简介），想要进一步了解该软件，请参阅 https://github.com/materialssimulation/HTMACat-kit


输入文件的准备
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

工作目录中需要准备好``config.yaml``文件
 
``Config.yaml`` 文件包含 **3** 个部分

* ``StuctInfo`` 部分
* ``Species`` 部分（可选）
* ``Model`` 部分

`config.yaml` 文件的典型构成如下:

.. code-block:: console

    StrucInfo:
      file: POSCARfile
      struct:
        element: Au
        lattice_type: fcc
        latttice_constant: 4.16
        facet: ['111','100'] #This is comment
        supercell: [3,3]     #Default: [3,3], can be undefined
        layers: 4            #Default: 4, can be undefined
        dope:
          Cu: [3]

    # This part alias for Species
    Species:
      file:
        'NH3+': NH3+.xyz #struct_file
        'NH3-': NH3-.xyz #struct_file
      sml:
        'O2': O=O #smiles

    Model:
      ads:
        - ['NH3+',1]
        - ['NO', 2]
        - [s: 'C(=O)O',1]
        - [f: 'NH3+',1]
        - [s: 'C[Al](C)C',1Al]
      coads:
        - ['NH3','O',1,1]

``StuctInfo`` 部分包含吸附基底的信息,
关键词存在两种形式: ``file`` 和 ``struct``

* ``file``: 路径，该关键词会从POSCAR文件中读取基底信息
* ``struct``: 使用该关键词会基于下列参数生成基底:
    * ``element``: bulk phase element
    * ``lattice_type``: lattice type
    * ``latticd_constant``: lattice parameter
    * ``facet``: crystal plane, should be a *list*, start with ``[``, separate with ``,`` and end with ``]``,
      facet index should be a str start with ``'`` end with ``'``, like ``'100'``, ``'111'``
    * ``supercell``: supercell of the substrate in the xy-plane
    * ``layers``: layers of the substrate in z axis
    * ``dope``: dope of the substrate, the formate is ``dope element : [dope type1, dope type2]``
      before ``:`` is the doped element, after the ``:`` is the dope type, the dope type can be
      chosen as follows: ``0`` corresponds to no doping, ``1``, ``2`` and ``3`` correspond to surface
      layers doped with ``1``, ``2`` and ``3`` atoms, respectively. ``1L`` and ``b1`` represent surface layer
      substitution and bulk equivalent proportional substitution.

``Species`` 部分为可选部分，用于物种的重命名:

* ``sml``: 使用 *Smiles* 格式表示吸附物种
* ``file``: 从结构文件中读取吸附物种，如 *xyz* 文件或 *vasp* 文件

``Model`` 部分包含吸附建模的参数:

* ``ads``: using ``- [ adsorbate formular , adsorption sites type]`` to represent one adsorption status, where
  adsorption formular should be a str start with ``'`` end with ``'``. Different adsorption status should start with
  new line, adsorption sites type can be chosen from ``1`` or ``2``.
* ``coads``: using ``- [ads1, ads2, ads1 sites, ads2 sites]``, ``ads1`` and ``ads2`` is two adsorbate species formular,
  ``ads1 sites`` and ``ads2 sites`` is adsorption sites type, can be chosen from ``1`` and ``2``.
* Each *adsorbate species formular* has three representation:
    * *str* format such as ``'ads1'``, this format will automatically generate species according to
      chemical formular
    * *smile* format such as ``s: 'ads1'`` or ``sml: 'ads1'``, this format will generate spcecies according to
      the *smile* formular
    * *file* format such as ``f: 'ads1'`` or ``file: 'ads1'``, this format will read structure from
      file 'ads1', now only 'xyz' and 'vasp' structure file format is supported
* New in version 1.0.5: Adding the symbol of the element contained in the adsorbate followed by the type of adsorption site 
  generates a configuration in which all atoms corresponding to that element are adsorbed atoms, and automatically adjusts the 
  orientation so that the adsorbed atom is at the bottom. Such as ``- [s: 'C[Al](C)C',1Al]`` will generatte configurations with 
  Al atoms as adsorbed stoms

**为了避免歧义，建议在声明复杂物种时使用SMILES**。
用户可根据自己的研究需要修改相应参数，实现个性化建模。

程序的运行
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~