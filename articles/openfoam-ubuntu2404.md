---
title: "Ubuntu 24.04でOpenFOAMを利用する"
emoji: "🦔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: 
  - OpenFOAM
  - CFD
  
published: false
---


こんにちは、ここあです。  
今回、流体解析ソフトウェアであるOpenCFDを導入したので、備忘録的にまとめていきます。  


## 環境
- OS: Ubuntu 24.04.1
- CPU: Core i7 8700K (6C/12T)
- RAM: 64GB 
- GPU: Nvidia GTX 1060

今回はGPUは関係ありませんが、後にAmgXやPyFRを用いて、GPGPUを利用した流体解析の計算支援をする目論見があるので記述しています。


## OpenFOAMの導入
基本的には公式に書かれている手順に従って導入します。  

https://openfoam.org/download/12-ubuntu/

```
sudo sh -c "wget -O - https://dl.openfoam.org/gpg.key > /etc/apt/trusted.gpg.d/openfoam.asc"
sudo add-apt-repository http://dl.openfoam.org/ubuntu
sudo apt update
sudo apt -y install openfoam12
```

続いて、OpenFOAMのパスをシステムに登録させます。  
`bash`を使っている人は `~/.bashrc` に、 `zsh` を使っている人は `~/.zshrc` に、それ以外の人も適宜読み替えて登録してください。  

```: ~/.zshrc
source /opt/openfoam12/etc/bashrc
```

最後に、下記のコマンドで設定を読み込みます。 

```
source ~/.zshrc
```

## 試運転
OpenFOAMをインストールすると、チュートリアルも一緒についてきます。  

``` bash
cp -r $FOAM_TUTORIALS/incompressibleFluid/pitzDailySteady .
cd pitzDailySteady
```

### メッシュの作成

初期条件などはすでに設定されているので、メッシュを切ります。

``` bash
blockMesh
```

### 解析の実行

続いて、解析を実行します。

```
foamRun
```

これにより、与えた条件に対する解析が実行されます。


:::: details 実行の様子
```
foamRun
/*---------------------------------------------------------------------------*\
  =========                 |
  \\      /  F ield         | OpenFOAM: The Open Source CFD Toolbox
   \\    /   O peration     | Website:  https://openfoam.org
    \\  /    A nd           | Version:  12
     \\/     M anipulation  |
\*---------------------------------------------------------------------------*/
Build  : 12-6aa359dae696
Exec   : foamRun
Date   : Nov 09 2024
Time   : 18:13:23
Host   : 
PID    : 2546438
I/O    : uncollated
Case   : /...../OpenFOAM_Foundation/pitzDailySteady
nProcs : 1
sigFpe : Enabling floating point exception trapping (FOAM_SIGFPE).
fileModificationChecking : Monitoring run-time modified files using timeStampMaster (fileModificationSkew 10)
allowSystemOperations : Allowing user-supplied system call operations

// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //
Create time

Create mesh for time = 0

Selecting solver incompressibleFluid
Selecting viscosity model constant
Selecting turbulence model type RAS
Selecting RAS turbulence model kEpsilon
Selecting generalised Newtonian model Newtonian
RAS
{
    model           kEpsilon;
    turbulence      on;
    printCoeffs     on;
    viscosityModel  Newtonian;
    Cmu             0.09;
    C1              1.44;
    C2              1.92;
    C3              0;
    sigmak          1;
    sigmaEps        1.3;
}

No MRF models present


SIMPLE: Convergence criteria found
        p: tolerance 0.01
        U: tolerance 0.001
        "(k|epsilon|omega|f|v2)": tolerance 0.001


SIMPLE: Operating solver in steady-state mode with 1 outer corrector
SIMPLE: Operating solver in SIMPLE mode



Starting time loop

streamlines streamlines:
    automatic track length specified through number of sub cycles : 5

streamlines streamlines write:
    Seeded 10 particles
    Sampled 114 locations
Time = 1s

smoothSolver:  Solving for Ux, Initial residual = 1, Final residual = 0.0538101, No Iterations 1
smoothSolver:  Solving for Uy, Initial residual = 1, Final residual = 0.030925, No Iterations 2
GAMG:  Solving for p, Initial residual = 1, Final residual = 0.068427, No Iterations 17
time step continuity errors : sum local = 1.19733, global = 0.179883, cumulative = 0.179883
smoothSolver:  Solving for epsilon, Initial residual = 0.232889, Final residual = 0.0114607, No Iterations 3
smoothSolver:  Solving for k, Initial residual = 1, Final residual = 0.045463, No Iterations 3
ExecutionTime = 0.203205 s  ClockTime = 0 s

....
....
....
....
Time = 283s

smoothSolver:  Solving for Ux, Initial residual = 0.000136028, Final residual = 1.32603e-05, No Iterations 5
smoothSolver:  Solving for Uy, Initial residual = 0.00107534, Final residual = 7.29428e-05, No Iterations 6
GAMG:  Solving for p, Initial residual = 0.00174139, Final residual = 0.000156972, No Iterations 3
time step continuity errors : sum local = 0.00663316, global = -0.000493114, cumulative = 0.714024
smoothSolver:  Solving for epsilon, Initial residual = 0.000145893, Final residual = 9.03593e-06, No Iterations 3
smoothSolver:  Solving for k, Initial residual = 0.000237046, Final residual = 1.44311e-05, No Iterations 4
ExecutionTime = 4.99165 s  ClockTime = 5 s

Time = 284s

smoothSolver:  Solving for Ux, Initial residual = 0.000131133, Final residual = 1.28406e-05, No Iterations 5
smoothSolver:  Solving for Uy, Initial residual = 0.00104952, Final residual = 7.08913e-05, No Iterations 6
GAMG:  Solving for p, Initial residual = 0.00157046, Final residual = 0.000128416, No Iterations 4
time step continuity errors : sum local = 0.00543223, global = 0.000631158, cumulative = 0.714656
smoothSolver:  Solving for epsilon, Initial residual = 0.0001417, Final residual = 8.85754e-06, No Iterations 3
smoothSolver:  Solving for k, Initial residual = 0.000229425, Final residual = 1.40595e-05, No Iterations 4
ExecutionTime = 5.00788 s  ClockTime = 5 s

Time = 285s

smoothSolver:  Solving for Ux, Initial residual = 0.000125234, Final residual = 1.23148e-05, No Iterations 5
smoothSolver:  Solving for Uy, Initial residual = 0.00100489, Final residual = 6.87913e-05, No Iterations 6
GAMG:  Solving for p, Initial residual = 0.00115928, Final residual = 8.12033e-05, No Iterations 5
time step continuity errors : sum local = 0.00342968, global = -0.000264336, cumulative = 0.714391
smoothSolver:  Solving for epsilon, Initial residual = 0.000136441, Final residual = 8.76233e-06, No Iterations 3
smoothSolver:  Solving for k, Initial residual = 0.000223256, Final residual = 1.36736e-05, No Iterations 4
ExecutionTime = 5.02501 s  ClockTime = 5 s

Time = 286s

smoothSolver:  Solving for Ux, Initial residual = 0.000121621, Final residual = 1.19474e-05, No Iterations 5
smoothSolver:  Solving for Uy, Initial residual = 0.000988614, Final residual = 6.70939e-05, No Iterations 6
GAMG:  Solving for p, Initial residual = 0.00134638, Final residual = 0.00010499, No Iterations 4
time step continuity errors : sum local = 0.0044293, global = 0.000501573, cumulative = 0.714893
smoothSolver:  Solving for epsilon, Initial residual = 0.000134768, Final residual = 8.78789e-06, No Iterations 3
smoothSolver:  Solving for k, Initial residual = 0.000218617, Final residual = 1.34194e-05, No Iterations 4
ExecutionTime = 5.04119 s  ClockTime = 5 s


SIMPLE solution converged in 286 iterations

streamlines streamlines write:
    Seeded 10 particles
    Sampled 12040 locations
End
```
::::

わたしの環境では5秒ほどで解析が終了しました。

解析結果を見るには、下記コマンドを実行します。  

```
paraFoam
```
![](/images/openfoam-ubuntu2404/001_single_analysis.png)

解析結果があっているのかわかりませんが、とりあえず結果が出ました。。。

## 並列演算
さて、今回のようなごく小規模の解析では気になりませんが、それなりの規模になってくると解析に時間がかかってきます。  
そこで、今回は並列に演算処理をすることで解析にかかる時間の短縮を狙ってみます。  

OpenFOAMは、どうやらOpenMPIを用いた並列演算をサポートしているようです。  
そのため、特別になにかをインストールすることなく、標準で並列演算をすることができます。

https://qiita.com/parapi_fluid/items/af2cdb4dcebfc940d874

### 分割設定
まずは、メッシュをコアに割り振るための設定を行います。  
今回は以下のように設定しました。

```: system/decomposeParDict
FoamFile
{
    version     2.0;
    format      ascii;
    class       dictionary;
    note        "mesh decomposition control dictionary";
    object      decomposeParDict;
}

// 分割数
numberOfSubdomains  4;

// 分割方式
method          simple;

// 分割
simpleCoeffs
{
    n           (2 2 1);  // 各方向での分割数 (例: 2x2x1) = 4
    delta       0.001;
}
```

`simple` での分割は、指定したブロックにメッシュを分割します。
この他にも、例えば　`scotch` では自動で均等な負荷になるように分割してくれるようです。

また、`numberOfSubdomains` はどうやらCPUの論理コア数を上限に決めると良さそうです。  
ただし、ただ単に上限にするのではなく、メッシュを切った後のセル数と相談し、配分後のセル数が小さくなりすぎないように調整すると良さそうです。  


### メッシュの割り当て

続いて、`decomposeParDict` に従い、メッシュを分割します。  
これには、下記コマンドを実行します。

```
decomposePar
```

今回は4分割をしたので、プロジェクトディレクトリには **processor0** ~ **processor3** までのディレクトリが生成されていると思います。

:::: details 分割後の様子

```
tree -L  2
.
├── 0
│    ├── epsilon
│    ├── f
│    ├── k
│    ├── nut
│    ├── nuTilda
│    ├── omega
│    ├── p
│    ├── U
│    └── v2
├── Allrun
├── constant
│    ├── momentumTransport
│    ├── physicalProperties
│    └── polyMesh
├── processor0
│    ├── 0
│    └── constant
├── processor1
│    ├── 0
│    └── constant
├── processor2
│    ├── 0
│    └── constant
├── processor3
│    ├── 0
│    └── constant
└── system
    ├── blockMeshDict
    ├── controlDict
    ├── decomposeParDict
    ├── functions
    ├── fvSchemes
    └── fvSolution
```
::::

### 解析の実行
続いて、解析を実行します。  
非分割時には例えば `foamRun` や `simpleFoam` などとしていたと思いますが、並列演算をする際には違ったコマンドを使用します。

```
mpirun -np 4 simpleFoam -parallel
```

というように実行します。

:::: details 実行の様子
```
mpirun -np 4 simpleFoam -parallel

/*---------------------------------------------------------------------------*\
  =========                 |
  \\      /  F ield         | OpenFOAM: The Open Source CFD Toolbox
   \\    /   O peration     | Website:  https://openfoam.org
    \\  /    A nd           | Version:  12
     \\/     M anipulation  |
\*---------------------------------------------------------------------------*/
Build  : 12-6aa359dae696
Exec   : foamRun -solver incompressibleFluid -parallel
Date   : Nov 09 2024
Time   : 18:40:08
Host   : 
PID    : 2564809
I/O    : uncollated
Case   : /....../OpenFOAM_Foundation/pitzDailySteady
nProcs : 4
Slaves : 
3
(
"......2564811"
"......2564813"
"......2564815"
)

Pstream initialised with:
    floatTransfer      : false
    nProcsSimpleSum    : 0
    commsType          : nonBlocking
    polling iterations : 0
sigFpe : Enabling floating point exception trapping (FOAM_SIGFPE).
fileModificationChecking : Monitoring run-time modified files using timeStampMaster (fileModificationSkew 10)
allowSystemOperations : Allowing user-supplied system call operations

// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //
Create time

Create mesh for time = 0

Selecting solver incompressibleFluid
Selecting viscosity model constant
Selecting turbulence model type RAS
Selecting RAS turbulence model kEpsilon
Selecting generalised Newtonian model Newtonian
RAS
{
    model           kEpsilon;
    turbulence      on;
    printCoeffs     on;
    viscosityModel  Newtonian;
    Cmu             0.09;
    C1              1.44;
    C2              1.92;
    C3              0;
    sigmak          1;
    sigmaEps        1.3;
}

No MRF models present


SIMPLE: Convergence criteria found
        p: tolerance 0.01
        U: tolerance 0.001
        "(k|epsilon|omega|f|v2)": tolerance 0.001


SIMPLE: Operating solver in steady-state mode with 1 outer corrector
SIMPLE: Operating solver in SIMPLE mode



Starting time loop

streamlines streamlines:
    automatic track length specified through number of sub cycles : 5

streamlines streamlines write:
    Seeded 10 particles
    Sampled 114 locations
Time = 1s

smoothSolver:  Solving for Ux, Initial residual = 1, Final residual = 0.0624406, No Iterations 1
smoothSolver:  Solving for Uy, Initial residual = 1, Final residual = 0.0345977, No Iterations 2
GAMG:  Solving for p, Initial residual = 1, Final residual = 0.0953916, No Iterations 15
time step continuity errors : sum local = 1.66915, global = 0.431908, cumulative = 0.431908
smoothSolver:  Solving for epsilon, Initial residual = 0.232906, Final residual = 0.011808, No Iterations 3
smoothSolver:  Solving for k, Initial residual = 1, Final residual = 0.0495972, No Iterations 3
ExecutionTime = 0.136607 s  ClockTime = 0 s

......
......
......

Time = 293s

smoothSolver:  Solving for Ux, Initial residual = 0.000123505, Final residual = 9.09438e-06, No Iterations 6
smoothSolver:  Solving for Uy, Initial residual = 0.00100191, Final residual = 7.68092e-05, No Iterations 6
GAMG:  Solving for p, Initial residual = 0.00186172, Final residual = 0.00011423, No Iterations 3
time step continuity errors : sum local = 0.00481771, global = -0.000671154, cumulative = 2.10619
smoothSolver:  Solving for epsilon, Initial residual = 0.000139385, Final residual = 1.00406e-05, No Iterations 3
smoothSolver:  Solving for k, Initial residual = 0.000227397, Final residual = 1.534e-05, No Iterations 4
ExecutionTime = 2.38129 s  ClockTime = 3 s

Time = 294s

smoothSolver:  Solving for Ux, Initial residual = 0.00012079, Final residual = 8.94887e-06, No Iterations 6
smoothSolver:  Solving for Uy, Initial residual = 0.000980767, Final residual = 7.38358e-05, No Iterations 6
GAMG:  Solving for p, Initial residual = 0.00104762, Final residual = 8.95664e-05, No Iterations 3
time step continuity errors : sum local = 0.00377283, global = -0.000373386, cumulative = 2.10582
smoothSolver:  Solving for epsilon, Initial residual = 0.000135936, Final residual = 9.93353e-06, No Iterations 3
smoothSolver:  Solving for k, Initial residual = 0.000219675, Final residual = 1.50131e-05, No Iterations 4
ExecutionTime = 2.38752 s  ClockTime = 3 s


SIMPLE solution converged in 294 iterations

streamlines streamlines write:
    Seeded 10 particles
    Sampled 12040 locations
End

Finalising parallel run
```
::::

わたしの環境では2 ~ 3秒で解析が終わりました。

### 結果の確認
分割して解析した場合、解析結果を見る前に結合するのを忘れないようにしましょう

```
reconstructPar
paraFoam
```


## さいごに
今回は導入して適当に遊んで終わりましたが、次はNvidiaが出しているAmgXと連携して、GPGPUを用いた解析をしてみたいと思います。
https://developer.nvidia.com/amgx

また別件で`PyFR`というCFDも触ってみたので、こちらについても導入をいつかまとめます。  
https://pyfr.readthedocs.io/en/latest/