**反应网络自动化生成**
======================================

反应网络自动化生成功能能够基于给定的反应体系，快速构建出候选的反应网络

一个典型的输入文件如下：

.. code-block:: console

    C2H6 -> C2H2 + H2       ! 总反应方程式（写法无严格要求）
    8                       ! 中间物种的最大原子数
    Reactants               ! 关键词，反应物（必须）
    CC                      ! 'Reactants'关键词下方的行：反应物的SMILES表达式
    Products                ! 关键词，产物（必须）
    C#C                     ! 'Products'关键词下方的行：产物的SMILES表达式
    [H][H]                  
    Settings                ! 关键词，设置（可选）
    exclude C               ! 可选功能'exclude'：生成的网络中强制不包含'C'物种
    include C=C             ! 可选功能'include'：生成的网络中必须包含'C=C'物种
    include [CH2-]C
    randomseed 0            ! 可选功能'randomseed'：为极小子图的搜索过程分配随机种子