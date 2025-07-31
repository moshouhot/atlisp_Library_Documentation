# atlisp 函数库使用指南 

## 目录

- [概述](#概述)
- [核心特性](#核心特性)
- [安装与配置](#安装与配置)
- [基本使用方法](#基本使用方法)
- [主要功能模块](#主要功能模块)
- [高级功能](#高级功能)
- [最佳实践](#最佳实践)
- [开发示例](#开发示例)
- [常见问题解答](#常见问题解答)
- [进阶技巧](#进阶技巧)
- [性能优化建议](#性能优化建议)
- [调试技巧](#调试技巧)
- [社区与支持](#社区与支持)
- [许可协议](#许可协议)
- [版本更新](#版本更新)
- [快速参考](#快速参考)


## 概述

atlisp 函数库是一个开源、共享、可云端加载的 AutoLISP 函数库，专为 AutoCAD、中望CAD、浩辰CAD 等兼容的 CAD 系统设计。它提供了上千个常用函数，涵盖图元操作、图块管理、实体对象、选择集、Excel 集成、剪贴板操作、曲线处理、颜色管理等各个方面，是 CAD 二次开发的强大工具库。

## 核心特性

### 🌐 云端加载
- **一行代码加载**：无需手动下载和配置，一行代码即可加载所需函数
- **自动更新**：函数库保持云端同步，始终使用最新版本
- **按需加载**：只加载需要的函数模块，提高效率

### 📚 丰富的函数库
- **1000+ 函数**：涵盖 CAD 开发的各个方面
- **57个分类**：按功能模块组织，便于查找和使用
- **开源共享**：遵循开放许可协议，可自由使用和贡献

### 🔧 易于集成
- **标准 AutoLISP**：完全兼容 AutoLISP 语法
- **跨平台支持**：支持多种 CAD 平台
- **无依赖安装**：通过 @lisp 应用管理器轻松安装

## 安装与配置

### 第一步：安装 @lisp 应用管理器

将以下代码复制到 CAD 命令行内，回车即可开始安装：

```lisp
(progn(vl-load-com)(setq s strcat h "http" o(vlax-create-object (s"win"h".win"h"request.5.1"))v vlax-invoke e eval r read)(v o'open "get" (s h"://atlisp.""cn/@"):vlax-true)(v o'send)(v o'WaitforResponse 1000)(e(r(vlax-get-property o'ResponseText))))
```

### 第二步：配置函数库

安装完成后，在 CAD 命令行输入以下命令：

- `@` - 重新加载 @lisp
- `@@` - 显示 @lisp 命令面板
- `@P` - 打开应用管理器
- `@H` - 查看帮助信息

## 基本使用方法

### 函数加载模式

@lisp 函数库支持两种加载模式：

#### 1. 云端实时加载（推荐）
```lisp
;; 加载单个函数
(load-function "base64:encode")

;; 加载整个模块
(load-module "string")
```

#### 2. 本地批量加载
```lisp
;; 加载所有函数到本地
(@lisp-load-all)
```

### 函数命名规范

atlisp 函数库采用模块化命名规范：

```
模块名:函数名
```

例如：
- `string:split` - 字符串分割函数
- `list:remove-duplicates` - 列表去重函数
- `entity:make-line` - 创建直线实体函数

## 主要功能模块

### 📝 字符串处理 (string)
```lisp
;; 字符串分割
(string:split "a,b,c" ",")  ; 返回 ("a" "b" "c")

;; 字符串替换
(string:subst-all "hello world" "o" "0")  ; 返回 "hell0 w0rld"

;; 数字格式化
(string:number-format 3.14159 2)  ; 返回 "3.14"

;; 字符串长度（支持中文）
(string:length "Hello世界")  ; 返回正确的字符数

;; 字符串反转
(string:reverse "hello")  ; 返回 "olleh"

;; 数字转中文
(string:number2chinese 123)  ; 返回 "一百二十三"
```

### 🔐 Base64 编码 (base64)
```lisp
;; Base64 编码
(base64:encode '(72 101 108 108 111))  ; 返回 "SGVsbG8="

;; Base64 解码
(base64:decode "SGVsbG8=")  ; 返回 (72 101 108 108 111)

;; 文件转 Base64
(base64:encode-from-file "C:\\test.txt")

;; Base64 转文件
(base64:base64-to-file "C:\\output.txt" "SGVsbG8=")
```

### 📋 列表操作 (list)
```lisp
;; 列表去重
(list:remove-duplicates '(1 2 2 3 3 3))  ; 返回 (1 2 3)

;; 列表交集
(list:intersection '(1 2 3) '(2 3 4))  ; 返回 (2 3)

;; 列表排序
(list:sort '(3 1 4 1 5) '<)  ; 返回 (1 1 3 4 5)
```

### 🎨 图元操作 (entity)
```lisp
;; 创建直线
(entity:make-line '(0 0 0) '(100 100 0))

;; 创建圆
(entity:make-circle '(0 0 0) 50)

;; 创建文字
(entity:make-text "Hello World" '(0 0 0) 10)
```

### 🔲 选择集处理 (pickset)
```lisp
;; 选择集转列表
(pickset:to-entlist ss)

;; 选择集排序
(pickset:sort ss)

;; 选择集过滤
(pickset:ssget "X" '((0 . "LINE")))
```

### 📊 Excel 集成 (excel)
```lisp
;; 打开 Excel 文件
(setq xl (excel:open "C:\\data.xlsx"))

;; 读取单元格
(excel:get-rangevalue xl "A1")

;; 写入单元格
(excel:set-rangevalue xl "A1" "Hello")

;; 保存并关闭
(excel:save xl)
(excel:quit xl)
```

### 🎯 几何计算 (geometry)
```lisp
;; 计算两点距离
(geometry:distance '(0 0 0) '(3 4 0))  ; 返回 5.0

;; 计算角度
(geometry:angle '(0 0 0) '(1 1 0))  ; 返回 π/4

;; 点到直线距离
(geometry:dist-pt-line pt line-start line-end)
```

## 高级功能

### 🔄 曲线处理 (curve)
```lisp
;; 多段线优化
(curve:optimize-lwpl ename)

;; 曲线长度计算
(curve:length ename)

;; 曲线中点
(curve:midpoint ename)

;; 三点求圆弧凸度
(curve:3pt2bulge pt1 pt2 pt3)

;; 检查弧线方向
(curve:clockwisep pt1 pt2 pt3)  ; 返回 T 表示顺时针

;; 圆转多段线
(curve:circle2lwpl center radius)
```

### 🕒 日期时间 (datetime)
```lisp
;; 获取当前时间
(datetime:current-time)  ; 返回当前时间字符串

;; 获取当前年份
(datetime:get-current-year)  ; 返回当前年份

;; 判断闰年
(datetime:leap-yearp 2024)  ; 返回 T 表示是闰年

;; 时间戳转换
(datetime:mktime year month day hour minute second)
```

### 🎨 颜色管理 (color)
```lisp
;; ACI 颜色转 RGB
(color:aci2rgb 1)  ; 返回红色 RGB 值 (255 0 0)

;; RGB 转真彩色
(color:rgb2truecolor 255 0 0)  ; 返回红色真彩色值

;; 真彩色转 RGB
(color:truecolor2rgb 16711680)  ; 返回 (255 0 0)

;; RGB 转 CSS 格式
(color:rgb2css 255 0 0)  ; 返回 "#FF0000"
```

### 🎨 颜色管理 (color)
```lisp
;; ACI 颜色转 RGB
(color:aci2rgb 1)  ; 返回红色 RGB 值

;; RGB 转真彩色
(color:rgb2truecolor 255 0 0)  ; 返回红色真彩色值
```

### 📁 文件操作 (file)
```lisp
;; 读取文件内容
(file:read-stream "C:\\test.txt")

;; 合并文件
(file:merge '("file1.txt" "file2.txt") "merged.txt")
```

## 最佳实践

### 1. 模块化开发
```lisp
;; 在程序开头加载所需模块
(load-module "string")
(load-module "list")
(load-module "entity")

;; 使用函数
(defun my-function ()
  (let ((text-list (string:split input-text ",")))
    (entity:make-text (car text-list) insert-point text-height)
  )
)
```

### 2. 错误处理
```lisp
(defun safe-function (/ result)
  (if (and (load-function "string:split")
           (setq result (string:split input ",")))
    result
    (progn
      (princ "\n函数加载失败或执行出错")
      nil
    )
  )
)
```

### 3. 性能优化
```lisp
;; 批量操作时，预先加载相关模块
(load-module "entity")
(load-module "pickset")

;; 使用局部变量减少重复计算
(let ((entities (pickset:to-entlist ss)))
  (mapcar 'entity:process-entity entities)
)
```

## 开发示例

### 示例1：批量文字处理
```lisp
(defun batch-text-process (/ ss ent-list)
  ;; 加载所需模块
  (load-module "pickset")
  (load-module "entity")
  (load-module "string")
  
  ;; 选择所有文字
  (setq ss (pickset:ssget "X" '((0 . "TEXT"))))
  (setq ent-list (pickset:to-entlist ss))
  
  ;; 处理每个文字
  (mapcar 
    '(lambda (ent)
       (let ((text (entity:getdxf ent 1)))
         (entity:putdxf ent 1 (string:case text "upper"))
       )
     )
    ent-list
  )
)
```

### 示例2：Excel 数据导入
```lisp
(defun import-from-excel (filename / xl data)
  ;; 加载 Excel 模块
  (load-module "excel")
  (load-module "entity")
  
  ;; 打开 Excel 文件
  (setq xl (excel:open filename))
  
  ;; 读取数据并创建图元
  (setq row 1)
  (while (setq data (excel:get-rangevalue xl (strcat "A" (itoa row))))
    (entity:make-text 
      data 
      (list (* row 100) 0 0) 
      10
    )
    (setq row (1+ row))
  )
  
  ;; 关闭 Excel
  (excel:quit xl)
)
```

## 进阶技巧

### 1. 自定义函数封装
```lisp
;; 封装常用操作为自定义函数
(defun my-create-layer (layer-name color / )
  (load-function "layer:make")
  (load-function "layer:set-color")
  (layer:make layer-name)
  (layer:set-color layer-name color)
)
```

### 2. 批处理模式
```lisp
;; 批量处理时关闭重绘提高性能
(defun batch-process (entity-list / old-regen)
  (setq old-regen (getvar "REGENMODE"))
  (setvar "REGENMODE" 0)

  ;; 批量处理
  (mapcar 'process-single-entity entity-list)

  ;; 恢复设置
  (setvar "REGENMODE" old-regen)
  (command "REGEN")
)
```

### 3. 错误恢复机制
```lisp
(defun robust-function (/ *error* old-error result)
  (setq old-error *error*)
  (setq *error*
    (lambda (msg)
      (princ (strcat "\n错误: " msg))
      (setq *error* old-error)
      (princ)
    )
  )

  ;; 主要逻辑
  (setq result (main-logic))

  ;; 恢复错误处理
  (setq *error* old-error)
  result
)
```

## 性能优化建议

### 1. 预加载策略
```lisp
;; 在程序开始时预加载所有需要的模块
(defun init-modules ()
  (mapcar 'load-module
    '("string" "list" "entity" "pickset" "geometry")
  )
)
```

### 2. 缓存机制
```lisp
;; 缓存计算结果避免重复计算
(setq *calculation-cache* (list))

(defun cached-calculation (input / result)
  (if (setq result (assoc input *calculation-cache*))
    (cdr result)
    (progn
      (setq result (expensive-calculation input))
      (setq *calculation-cache*
        (cons (cons input result) *calculation-cache*)
      )
      result
    )
  )
)
```

### 3. 内存管理
```lisp
;; 及时释放大型数据结构
(defun process-large-data (data / result)
  (setq result (process data))
  (setq data nil)  ; 释放内存
  result
)
```

## 调试技巧

### 1. 调试输出
```lisp
(defun debug-print (msg value)
  (princ (strcat "\n[DEBUG] " msg ": "))
  (prin1 value)
  (princ)
  value
)

;; 使用示例
(setq result (debug-print "计算结果" (+ 1 2 3)))
```

### 2. 函数跟踪
```lisp
(defun trace-function (func-name)
  (princ (strcat "\n[TRACE] 调用函数: " func-name))
  (princ)
)
```

### 3. 性能监控
```lisp
(defun time-function (func / start-time end-time)
  (setq start-time (getvar "MILLISECS"))
  (setq result (func))
  (setq end-time (getvar "MILLISECS"))
  (princ (strcat "\n执行时间: "
    (rtos (- end-time start-time) 2 2) " 毫秒"))
  result
)
```

## 快速参考

### 常用命令速查
| 命令 | 功能 | 说明 |
|------|------|------|
| `@` | 重新加载 atlisp | 修复或更新系统 |
| `@@` | 显示命令面板 | 图形界面操作 |
| `@P` | 应用管理器 | 管理应用包 |
| `@H` | 帮助信息 | 查看使用说明 |
| `@I` | 安装应用包 | 命令行安装 |
| `@L` | 列出应用包 | 查看已安装包 |
| `@U` | 升级系统 | 更新到最新版 |
| `@W` | 访问网站 | 打开官网 |

### 函数模块速查
| 模块 | 主要功能 | 常用函数示例 |
|------|----------|--------------|
| `string` | 字符串处理 | `split`, `subst-all`, `length` |
| `list` | 列表操作 | `remove-duplicates`, `sort`, `intersection` |
| `entity` | 图元操作 | `make-line`, `make-circle`, `getdxf` |
| `pickset` | 选择集处理 | `to-entlist`, `sort`, `ssget` |
| `excel` | Excel 集成 | `open`, `get-rangevalue`, `save` |
| `geometry` | 几何计算 | `distance`, `angle`, `midpoint` |
| `curve` | 曲线处理 | `length`, `optimize-lwpl`, `midpoint` |
| `color` | 颜色管理 | `aci2rgb`, `rgb2truecolor` |
| `layer` | 图层管理 | `make`, `set-color`, `freeze` |
| `block` | 图块操作 | `insert`, `get-attributes`, `list` |

### 错误代码速查
| 错误信息 | 可能原因 | 解决方法 |
|----------|----------|----------|
| 函数未定义 | 函数未加载 | 检查函数名，重新加载模块 |
| 网络连接失败 | 网络问题 | 检查网络，稍后重试 |
| 参数错误 | 参数类型不匹配 | 检查参数类型和数量 |
| 权限不足 | 文件访问权限 | 以管理员身份运行 CAD |

---


# atlisp 函数库完整文档

本文档包含 atlisp 函数库的完整使用说明，适用于 AutoLISP 开发。

## 📊 统计信息

- **总函数数量**: 827
- **功能分类**: 57
- **文档生成时间**: 2025-07-31 15:35:47

## 🌟 概述

atlisp 是一个功能强大的 AutoLISP 函数库，提供了丰富的工具函数来简化 AutoCAD 开发。
本库采用云端加载模式，支持按需加载，包含以下主要功能模块：

### 🔥 主要功能模块

1. **vectra** - 152 个函数
2. **list** - 62 个函数
3. **entity** - 61 个函数
4. **m** - 56 个函数
5. **curve** - 45 个函数
6. **string** - 38 个函数
7. **excel** - 33 个函数
8. **pickset** - 30 个函数
9. **std** - 29 个函数
10. **dcl** - 24 个函数

## 📚 函数分类目录

| 分类 | 函数数量 | 主要功能 |
|------|----------|----------|
| [base](#base) | 1 | 专用功能模块 |
| [base64](#base64) | 4 | Base64编码解码 |
| [block](#block) | 22 | 图块操作、属性处理 |
| [cl](#cl) | 6 | 专用功能模块 |
| [clipboard](#clipboard) | 4 | 专用功能模块 |
| [color](#color) | 8 | 颜色管理、格式转换 |
| [crypto](#crypto) | 1 | 专用功能模块 |
| [curve](#curve) | 45 | 曲线处理、长度计算、优化 |
| [datetime](#datetime) | 9 | 日期时间处理、格式化 |
| [dbx](#dbx) | 3 | 专用功能模块 |
| [dcl](#dcl) | 24 | 专用功能模块 |
| [demo](#demo) | 1 | 专用功能模块 |
| [entity](#entity) | 61 | 图元创建、修改、查询操作 |
| [env](#env) | 2 | 专用功能模块 |
| [example](#example) | 17 | 专用功能模块 |
| [excel](#excel) | 33 | Excel文件读写、数据处理 |
| [ext](#ext) | 4 | 专用功能模块 |
| [file](#file) | 6 | 文件操作、读写处理 |
| [filename](#filename) | 2 | 专用功能模块 |
| [fun](#fun) | 1 | 专用功能模块 |
| [g](#g) | 2 | 专用功能模块 |
| [geometry](#geometry) | 15 | 几何计算、坐标变换 |
| [group](#group) | 6 | 专用功能模块 |
| [hdinfo](#hdinfo) | 3 | 专用功能模块 |
| [ini](#ini) | 5 | 专用功能模块 |
| [json](#json) | 4 | 专用功能模块 |
| [layer](#layer) | 23 | 图层管理、属性设置 |
| [layout](#layout) | 7 | 专用功能模块 |
| [line](#line) | 3 | 专用功能模块 |
| [list](#list) | 62 | 列表操作、排序、去重、查找 |
| [m](#m) | 56 | 数学运算、三角函数 |
| [matrix](#matrix) | 20 | 矩阵运算、变换计算 |
| [music-die](#musicdie) | 3 | 专用功能模块 |
| [orance](#orance) | 1 | 专用功能模块 |
| [p](#p) | 19 | 专用功能模块 |
| [pickset](#pickset) | 30 | 选择集处理、过滤、转换 |
| [point](#point) | 9 | 点坐标处理、变换 |
| [py](#py) | 1 | 专用功能模块 |
| [re](#re) | 3 | 专用功能模块 |
| [reg](#reg) | 1 | 专用功能模块 |
| [stat](#stat) | 5 | 专用功能模块 |
| [std](#std) | 29 | 专用功能模块 |
| [string](#string) | 38 | 字符串处理、格式化、编码转换 |
| [style](#style) | 1 | 专用功能模块 |
| [svg](#svg) | 1 | 专用功能模块 |
| [sys](#sys) | 2 | 专用功能模块 |
| [system](#system) | 11 | 系统信息、环境变量 |
| [table](#table) | 3 | 专用功能模块 |
| [tbl](#tbl) | 3 | 专用功能模块 |
| [text](#text) | 10 | 专用功能模块 |
| [timer](#timer) | 2 | 专用功能模块 |
| [ui](#ui) | 17 | 用户界面、对话框 |
| [vectra](#vectra) | 152 | 专用功能模块 |
| [vitalgg](#vitalgg) | 2 | 专用功能模块 |
| [vla](#vla) | 13 | 专用功能模块 |
| [word](#word) | 7 | 专用功能模块 |
| [xdata](#xdata) | 4 | 专用功能模块 |

## base

**模块说明**: 专用功能模块

### base:init

**说明**: atlisp 函数库基本符号及简化函数。为了保证atlisp函数库能正常运行，需先执行该函数。该函数被 atlisp 自动调用。

**用法**:
```lisp
(base:init )
```

**参数**: 没有

**示例**:
```lisp
(base:init)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun base:init nil "atlisp 函数库基本符号及简化函数。为了保证atlisp函数库能正常运行，需先执行该函数。该函数被 atlisp 自动调用。"
  ""
  "(base:init)"
  "常用 visuallisp 全局符号。"
  (or (eq $platform (quote linux))
    (setq *acad* (vlax-get-acad-object)
      *doc* (vla-get-activedocument *acad*)
      *docs* (vla-get-documents *acad*)
      *ms* (vla-get-modelspace *doc*)
      *ps* (vla-get-paperspace *doc*)
      *blks* (vla-get-blocks *doc*)
      *lays* (vla-get-layers *doc*)
      *lts* (vla-get-linetypes *doc*)
      *sts* (vla-get-textstyles *doc*)
      *grps* (vla-get-groups *doc*)
      *dims* (vla-get-dimstyles *doc*)
      *louts* (vla-get-layouts *doc*)
      *vps* (vla-get-viewports *doc*)
      *vs* (vla-get-views *doc*)
      *dics* (vla-get-dictionaries *doc*)
      *layouts* (vla-get-layouts *doc*)
      *display* (vla-get-display (vla-get-preferences (vla-get-application *acad*)))))
  "简化函数"
  (setq o2e vlax-vla-object->ename)
  (setq e2o vlax-ename->vla-object)
  (setq vlap p:vlap stringp p:stringp realp p:realp enamep p:enamep variantp p:variantp picksetp p:picksetp intp p:intp safearrayp p:safearrayp ename-listp p:ename-listp vla-listp p:vla-listp string-listp p:string-listp dotpairp p:dotpairp curvep p:curvep)
  (setq @:*units* (list "m"
      "kg"
      "s"
      "h"
      "A"
      "K"
      "mol"
      "cd"
      (setq m2 (strcat "m"
          (chr 178)))
      (setq m3 (strcat "m"
          (chr 179)))
      (strcat m3 "/mol")
      (strcat m3 "/kg")
      "Hz"
      (strcat "kg/"
        m3)
      "kg/mol"
      "m/s"
      "rad/s"
      "N"
      "Pa"
      "N/m"
      "N·s"
      "J"
      "N·m"
      "J/mol"
      "W"
      "J/s"
      "J/K"
      "J/(mol·K)"
      "J/(kg·K)"
      (strcat "N·s/"
        m2)
      "W/(m·K)"
      (strcat m2 "/s")
      "C"
      "V"
      "Ω"
      "m"
      "mm"
      "cm"
      "dm"
      "km"
      "g"
      "mg"
      "k"
      "M"
      "kN"
      "MPa"
      "kPa"
      "dB"
      "℃"
      "ppm"
      "人"
      (chr 178)
      (chr 179)
      (chr 181)
      "/"
      "·"
      "("
        ")"
      (strcat "W/"
        m2 "·K")
      "℉"
      "Bq"
      "lx"
      "lm"
      "S"
      "H"
      "Wb"
      "T"
      "F"
      "W"))
  (setq *linefile* (cond (is-bricscad "default.lin")
      (is-gcad "gcad.lin")
      (is-zwcad "zwcad.lin")
      (t "acad.lin")))
  (setq *isolinefile* (cond (is-bricscad "iso.lin")
      (is-gcad "gcadiso.lin")
      (is-zwcad "zwcadiso.lin")
      (t "acadiso.lin")))
  (setq *patfile* (cond (is-bricscad "default.pat")
      (is-gcad "gcad.pat")
      (is-zwcad "zwcad.pat")
      (t "acad.pat")))
  (setq *isopatfile* (cond (is-bricscad "iso.pat")
      (is-gcad "gcadiso.pat")
      (is-zwcad "zwcadiso.pat")
      (t "acadiso.pat")))
  t)
```
</details>

---

## base64

**模块说明**: Base64编码解码

### base64:base64-to-file

**用法**:
```lisp
(base64:base64-to-file file str-base64)
```

**参数**: 1 file  : 文件名;2 str-base64  : 字符串;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun base64:base64-to-file (file str-base64 / xmldom node stream)
  (if is-gcad (progn (princ "This function CAN NOT support GstarCAD.")
      nil)
    (progn (setq xmldom (vlax-get-or-create-object "MSXML2.DOMDocument"))
      (setq node (vlax-invoke-method xmldom (quote createelement)
          "binary"))
      (vlax-put-property node (quote datatype)
        "bin.base64")
      (vlax-put-property node (quote text)
        str-base64)
      (setq stream (vlax-get-or-create-object "ADODB.Stream"))
      (vlax-put-property stream (quote type)
        1)
      (vlax-invoke stream (quote open))
      (vlax-invoke-method stream (quote write)
        (vlax-get-property node (quote nodetypedvalue)))
      (vlax-invoke-method stream (quote savetofile)
        file 2)
      (vlax-invoke stream (quote close))
      (vlax-release-object stream)
      (vlax-release-object node)
      (vlax-release-object xmldom)
      t)))
```
</details>

---

### base64:decode

**说明**: Decode base64 string.

**用法**:
```lisp
(base64:decode str-base64)
```

**参数**: 1 str-base64  : 字符串;

**返回值**: list of unsigned 8bit integer.

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun base64:decode (str-base64 / i res lst-base64 h%)
  "Decode base64 string."
  "list of unsigned 8bit integer."
  (if (p:stringp str-base64)
    (progn (setq lst-base64 (vl-string->list "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"))
      (setq i 0)
      (setq res (quote nil))
      (setq h% 0)
      (foreach b (mapcar (quote (lambda (x)
              (vl-position x lst-base64)))
          (vl-string->list str-base64))
        (if b (cond ((= i 0)
              (setq h% b)
              (setq i 1))
            ((= i 1)
              (setq res (cons (+ (lsh h% 2)
                    (/ b 16))
                  res))
              (setq h% (logand b 15))
              (setq i 2))
            ((= i 2)
              (setq res (cons (+ (lsh h% 4)
                    (/ b 4))
                  res))
              (setq h% (logand b 2))
              (setq i 3))
            ((= i 3)
              (setq res (cons (+ (lsh h% 6)
                    b)
                  res))
              (setq h% 0)
              (setq i 0)))))
      (reverse res))))
```
</details>

---

### base64:encode

**说明**: 将字节列表内容转为 base64 编码。

**用法**:
```lisp
(base64:encode lst-uint8)
```

**参数**: 1 lst-uint8  : 列表;

**返回值**: String

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun base64:encode (lst-uint8 / i res lst-str rem%)
  "将字节列表内容转为 base64 编码。"
  "String"
  (if (and (listp lst-uint8)
      (apply (quote and)
        (mapcar (quote numberp)
          lst-uint8)))
    (progn (setq lst-base64 (vl-string->list "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"))
      (setq i 0)
      (setq res (quote nil))
      (setq rem% 0)
      (foreach b lst-uint8 (cond ((= i 0)
            (setq res (cons (/ b 4)
                res))
            (setq rem% (logand b 3))
            (setq i 1))
          ((= i 1)
            (setq res (cons (+ (lsh rem% 4)
                  (/ b 16))
                res))
            (setq rem% (logand b 15))
            (setq i 2))
          ((= i 2)
            (setq res (cons (+ (lsh rem% 2)
                  (/ b 64))
                res))
            (setq res (cons (logand b 63)
                res))
            (setq rem% 0)
            (setq i 0))))
      (cond ((= i 1)
          (setq res (cons (lsh rem% 4)
              res)))
        ((= i 2)
          (setq res (cons (lsh rem% 2)
              res))))
      (setq res (mapcar (quote (lambda (x)
              (nth x lst-base64)))
          res))
      (if (/= i 0)
        (repeat (- 3 i)
          (setq res (cons (ascii "=")
              res))))
      (vl-list->string (reverse res)))))
```
</details>

---

### base64:encode-from-file

**说明**: 将文件 file 转为 base64 编码。文件过大会转换失败。

**用法**:
```lisp
(base64:encode-from-file file)
```

**参数**: 1 file  : 文件名;

**返回值**: String

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun base64:encode-from-file (file / fp b)
  "将文件 file 转为 base64 编码。文件过大会转换失败。"
  "String"
  ""
  (princ "CAN NOT read long file.\n")
  (if (and (findfile file)
      (setq fp (open file "r")))
    (progn (setq lst-base64 (vl-string->list "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"))
      (setq i 0)
      (setq res (quote nil))
      (setq rem% 0)
      (while (setq b (read-char fp))
        (cond ((= i 0)
            (setq res (cons (/ b 4)
                res))
            (setq rem% (logand b 3))
            (setq i 1))
          ((= i 1)
            (setq res (cons (+ (lsh rem% 4)
                  (/ b 16))
                res))
            (setq rem% (logand b 15))
            (setq i 2))
          ((= i 2)
            (setq res (cons (+ (lsh rem% 2)
                  (/ b 64))
                res))
            (setq res (cons (logand b 63)
                res))
            (setq rem% 0)
            (setq i 0))))
      (close fp)
      (cond ((= i 1)
          (setq res (cons (lsh rem% 4)
              res)))
        ((= i 2)
          (setq res (cons (lsh rem% 2)
              res))))
      (setq res (mapcar (quote (lambda (x)
              (nth x lst-base64)))
          res))
      (if (/= i 0)
        (repeat (- 3 i)
          (setq res (cons (ascii "=")
              res))))
      (reverse res))))
```
</details>

---

## block

**模块说明**: 图块操作、属性处理

### block:attach

**说明**: 附着外部参照。浩辰CAD,中望CAD以块方式插入。

**用法**:
```lisp
(block:attach path pt ang scale)
```

**参数**: 1 path  : 文件路径;2 pt  : 单个2D/3D坐标点;3 ang  : 角度值;4 scale  : 比例值;

**示例**:
```lisp
(block:attach "D:/a.dwg" (getpoint) 0 1)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:attach (path pt ang scale)
  "附着外部参照。浩辰CAD,中望CAD以块方式插入。"
  ""
  "(block:attach \"D:/a.dwg\"
    (getpoint)
    0 1)"
  (cond ((or is-gcad is-zwcad (< (@:acadver)
          18))
      (block:insert (vl-filename-base path)
        path pt ang scale))
    ((>= (@:acadver)
        18)
      (setvar "cmdecho"
        0)
      (command "-attach"
        path "A"
        pt scale scale ang)
      (setvar "cmdecho"
        1)))
  (entlast))
```
</details>

---

### block:bcs2wcs

**说明**: 将块定义内的坐标转为块引用中的实际的世界坐标

**用法**:
```lisp
(block:bcs2wcs pt pt-base pt-ins ang scale)
```

**参数**: 1 pt  : 单个2D/3D坐标点;2 pt-base  : 单个2D/3D坐标点;3 pt-ins  : 单个2D/3D坐标点;4 ang  : 角度值;5 scale  : 比例值;

**返回值**: 坐标值

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:bcs2wcs (pt pt-base pt-ins ang scale)
  "将块定义内的坐标转为块引用中的实际的世界坐标"
  "坐标值"
  (setq pt (mapcar (quote -)
      pt pt-base))
  (m:coordinate pt-ins (m:coordinate-scale (m:coordinate-rotate pt ang)
      scale)))
```
</details>

---

### block:ent-list

**说明**: 返回块内各图元的列表

**用法**:
```lisp
(block:ent-list blkname)
```

**参数**: 1 blkname  : 块名称;

**返回值**: 图元列表

**示例**:
```lisp
(block:ent-list "图框")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:ent-list (blkname / blk)
  "返回块内各图元的列表"
  "图元列表"
  "(block:ent-list \"图框\")"
  (if (setq blk (tblobjname "block"
        blkname))
    (progn (setq ent blk)
      (setq entlst nil)
      (while (setq ent (entnext ent))
        (setq entlst (cons ent entlst)))
      (reverse entlst))))
```
</details>

---

### block:get-attdef

**说明**: 取块定义中的属性定义名及提示名

**用法**:
```lisp
(block:get-attdef blkname)
```

**参数**: 1 blkname  : 块名称;

**返回值**: list

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:get-attdef (blkname)
  "取块定义中的属性定义名及提示名"
  "list"
  (setq ents (vl-remove-if-not (quote (lambda (x)
          (equal "ATTDEF"
            (entity:getdxf x 0))))
      (block:ent-list blkname)))
  (mapcar (quote (lambda (x)
        (entity:getdxf x (quote (2 3)))))
    ents))
```
</details>

---

### block:get-attrib-ents

**说明**: 获取块参照的属性图元。

**用法**:
```lisp
(block:get-attrib-ents blkref)
```

**参数**: 1 blkref  : 块图元/对象;

**返回值**: 图元列表

**示例**:
```lisp
(block:get-attrib-ents (car(entsel)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:get-attrib-ents (blkref / lst)
  "获取块参照的属性图元。"
  "图元列表"
  "(block:get-attrib-ents (car(entsel)))"
  (if (= (quote vla-object)
      (type blkref))
    (setq blkref (o2e blkref)))
  (if (and (= (quote ename)
        (type blkref))
      (eq "INSERT"
        (entity:getdxf blkref 0))
      (= 1 (entity:getdxf blkref 66)))
    (progn (setq att (entnext blkref))
      (while (eq "ATTRIB"
          (entity:getdxf att 0))
        (setq lst (cons att lst))
        (setq att (entnext att)))
      (reverse lst))))
```
</details>

---

### block:get-attributes

**说明**: 获取块属性,返回属性名和值的点对列表。

**用法**:
```lisp
(block:get-attributes blkref)
```

**参数**: 1 blkref  : 块图元/对象;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:get-attributes (blkref / lst)
  "获取块属性,返回属性名和值的点对列表。"
  (if (= (quote ename)
      (type blkref))
    (setq blkref (e2o blkref)))
  (if (p:functionp vla-getattributes)
    (if (= (quote vla-object)
        (type blkref))
      (if (safearray-value (setq lst (vlax-variant-value (vla-getattributes blkref))))
        (mapcar (quote (lambda (x)
              (cons (vla-get-tagstring x)
                (vla-get-textstring x))))
          (vlax-safearray->list lst)))
      nil)
    (progn (setq att (entnext blkref))
      (while (eq "ATTRIB"
          (entity:getdxf att 0))
        (setq lst (cons (cons (entity:getdxf att 2)
              (entity:getdxf att 1))
            lst))
        (setq att (entnext att)))
      (reverse lst))))
```
</details>

---

### block:get-clip-boundary

**说明**: 取剪裁块参照的边界线点WCS坐标

**用法**:
```lisp
(block:get-clip-boundary blkref)
```

**参数**: 1 blkref  : 块图元/对象;

**返回值**: list

**示例**:
```lisp
(block:get-clip-boundary (car(ensel)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:get-clip-boundary (blkref / ent-sf boundary-in-blk mt ms mr)
  "取剪裁块参照的边界线点WCS坐标"
  "list"
  "(block:get-clip-boundary (car(ensel)))"
  (if (setq ent-sf (entity:getdxf (entity:getdxf (entity:getdxf blkref 360)
          360)
        360))
    (progn (setq matrixs (list:split (entity:getdxf ent-sf 40)
          12))
      (setq pts (entity:getdxf ent-sf 10))
      (if (= 2 (length pts))
        (setq pts (apply (quote point:rec-2pt->4pt)
            pts)))
      (setq boundary-in-blk (mapcar (quote (lambda (x)
              (matrix:mxp (list:split (car matrixs)
                  4)
                x)))
          pts))
      (setq mt (apply (quote matrix:translation)
          (entity:getdxf blkref 10)))
      (setq ms (matrix:scale (entity:getdxf blkref 41)
          (entity:getdxf blkref 42)
          (entity:getdxf blkref 43)))
      (setq mr (matrix:rotation-z (- (* 2 pi)
            (entity:getdxf blkref 50))))
      (mapcar (quote (lambda (x)
            (matrix:transform mt ms mr x)))
        boundary-in-blk))))
```
</details>

---

### block:get-dynamic-prop-cons-name-value

**说明**: 获取动态块的动态特性与值的点对列表。

**用法**:
```lisp
(block:get-dynamic-prop-cons-name-value blkref)
```

**参数**: 1 blkref  : 块图元/对象;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:get-dynamic-prop-cons-name-value (blkref / props n lst)
  "获取动态块的动态特性与值的点对列表。"
  (setq props (block:get-dynamic-properties blkref))
  (setq n 0)
  (setq lst nil)
  (repeat (length (car props))
    (setq lst (append lst (list (cons (nth n (car props))
            (nth n (cadr props))))))
    (setq n (1+ n)))
  lst)
```
</details>

---

### block:get-dynamic-properties

**说明**: 获取动态块的动态特性(自定义)列表：特性名，当前值，只读性，是否显示，允许值

**用法**:
```lisp
(block:get-dynamic-properties blk)
```

**参数**: 1 blk  : 块图元/对象;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:get-dynamic-properties (blk / oblk props)
  "获取动态块的动态特性(自定义)列表：特性名，当前值，只读性，是否显示，允许值"
  (if (= (quote ename)
      (type blk))
    (progn (setq oblk (vlax-ename->vla-object blk))
      (setq props (vlax-invoke oblk (quote getdynamicblockproperties)))
      (list (mapcar (quote (lambda (x)
              (vlax-get-property x (quote propertyname))))
          props)
        (mapcar (quote (lambda (x)
              (vlax-get-property x (quote value))))
          props)
        (mapcar (quote vla-get-readonly)
          props)
        (mapcar (quote vla-get-show)
          props)
        (mapcar (quote (lambda (x)
              (vlax-get-property x (quote allowedvalues))))
          props)))
    nil))
```
</details>

---

### block:get-dynprop

**说明**: 取动态块的某一特性的值

**用法**:
```lisp
(block:get-dynprop blkref prp)
```

**参数**: 1 blkref  : 块图元/对象;2 prp  : 块特性;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:get-dynprop (blkref prp)
  "取动态块的某一特性的值"
  (cdr (assoc prp (block:get-properties blkref))))
```
</details>

---

### block:get-effectivename

**说明**: 取得块真实名称，支持 MAC

**用法**:
```lisp
(block:get-effectivename blkref)
```

**参数**: 1 blkref  : 块图元/对象;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:get-effectivename (blkref / tem blkname *error*)
  "取得块真实名称，支持 MAC"
  (defun *error* (msg)
    (princ "图块定义异常")
    "")
  (cond ((and (= (quote ename)
          (type blkref))
        (entget blkref))
      (progn (setq blkname (cdr (assoc 2 (entget blkref))))
        (if (wcmatch blkname "`**")
          (if (and (setq tem (cdadr (assoc -3 (entget (cdr (assoc 330 (entget (tblobjname "block"
                              blkname))))
                      (quote ("AcDbBlockRepBTag"))))))
              (setq tem (handent (cdr (assoc 1005 tem)))))
            (setq blkname (cdr (assoc 2 (entget tem))))))
        blkname))
    ((= (quote vla-object)
        (type blkref))
      (vla-get-effectivename blkref))
    (t "")))
```
</details>

---

### block:get-obj-by-name

**说明**: 通过块名获取块的AX对象

**用法**:
```lisp
(block:get-obj-by-name blk-name)
```

**参数**: 1 blk-name  : 块图元/对象;

**返回值**: vla-object

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:get-obj-by-name (blk-name)
  "通过块名获取块的AX对象"
  "vla-object"
  (car (vl-remove-if-not (quote (lambda (x)
          (= (vla-get-name x)
            blk-name)))
      (block:list-blk-objs))))
```
</details>

---

### block:get-properties

**说明**: 获取动态块的动态特性和值的点对表

**用法**:
```lisp
(block:get-properties blkref)
```

**参数**: 1 blkref  : 块图元/对象;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:get-properties (blkref / oblkref)
  "获取动态块的动态特性和值的点对表"
  (if (= (quote ename)
      (type blkref))
    (mapcar (function (lambda (prop)
          (cons (vla-get-propertyname prop)
            (vla:get-value (vla-get-value prop)))))
      (vlax-invoke (e2o blkref)
        (quote getdynamicblockproperties)))))
```
</details>

---

### block:insert

**说明**: 插入块参照，参数：blkname 块名，path 块文件路径(以/结尾,不含块文件名)， pt 插入点, ang 旋转角度，scale 比例。

**用法**:
```lisp
(block:insert blkname path pt ang scale)
```

**参数**: 1 blkname  : 块名称;2 path  : 文件路径;3 pt  : 单个2D/3D坐标点;4 ang  : 角度值;5 scale  : 比例值;

**返回值**: 块实体

**示例**:
```lisp
(block:insert "aa" "C:/design/" (getpoint) 0 1)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:insert (blkname path pt ang scale)
  "插入块参照，参数：blkname 块名，path 块文件路径(以/结尾,不含块文件名)， pt 插入点, ang 旋转角度，scale 比例。"
  "块实体"
  "(block:insert \"aa\"
    \"C:/design/\"
    (getpoint)
    0 1)"
  (if (tblsearch "block"
      blkname)
    (progn (if vla-insertblock (vla-insertblock (if (string-equal "Model"
              (getvar "ctab"))
            *ms* (vla-get-block (vla-get-activelayout *doc*)))
          (point:to-ax pt)
          blkname scale scale scale ang)
        (entmakex (list (quote (0 . "INSERT"))
            (quote (100 . "AcDbEntity"))
            (quote (100 . "AcDbBlockReference"))
            (cons 2 blkname)
            (cons 10 pt)
            (cons 41 scale)
            (cons 42 scale)
            (cons 43 scale)
            (cons 50 ang))))
      (entlast))
    (if (findfile (strcat path blkname ".dwg"))
      (progn (if vla-insertblock (vla-insertblock (if (string-equal "Model"
                (getvar "ctab"))
              *ms* (vla-get-block (vla-get-activelayout *doc*)))
            (point:to-ax pt)
            (findfile (strcat path blkname ".dwg"))
            scale scale scale ang)
          (progn (setvar "attreq"
              0)
            (command "-insert"
              (strcat path blkname)
              pt scale scale (angtos ang 0 0))
            (setvar "attreq"
              1)))
        (entlast)))))
```
</details>

---

### block:list

**说明**: 列块的名称

**用法**:
```lisp
(block:list )
```

**参数**: 没有一个

**返回值**: list

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:list (/ res name)
  "列块的名称"
  "list"
  (if (setq name (cdr (assoc 2 (tblnext "block"
            t))))
    (setq res (cons name nil)))
  (while (setq name (tblnext "block"))
    (setq res (cons (cdr (assoc 2 name))
        res)))
  res)
```
</details>

---

### block:list-blk-objs

**说明**: 获取块对象列表

**用法**:
```lisp
(block:list-blk-objs )
```

**参数**: None

**返回值**: 获取块及外部参照对象列表

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:list-blk-objs (/ res)
  "获取块对象列表"
  "获取块及外部参照对象列表"
  ""
  (setq res nil)
  (vlax-for blk *blks* (setq res (cons blk res)))
  res)
```
</details>

---

### block:list-xref-objs

**说明**: 获取外部参照对象列表

**用法**:
```lisp
(block:list-xref-objs )
```

**参数**: None

**返回值**: 外部参照对象列表

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:list-xref-objs (/ res)
  "获取外部参照对象列表"
  "外部参照对象列表"
  ""
  (setq res nil)
  (vlax-for blk *blks* (if (= :vlax-true (vla-get-isxref blk))
      (setq res (cons blk res))))
  res)
```
</details>

---

### block:readme

**说明**: 图块及外部参照操作相关函数。

**用法**:
```lisp
(block:readme )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:readme nil "图块及外部参照操作相关函数。"
  (princ "图块及外部参照操作相关函数。使用 (require 'block:*)
    加载这些函数")
  (princ))
```
</details>

---

### block:set-attributes

**说明**: 设置块属性值，blkref 为块参照的图元名，lst 为多个属性名和属性值组成的点对表。

**用法**:
```lisp
(block:set-attributes blkref lst)
```

**参数**: 1 blkref  : 块图元/对象;2 lst  : 列表;

**示例**:
```lisp
(block:set-attributes blkref '(("att1" . "value1")("att2" . "value2")))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:set-attributes (blkref lst / n atts)
  "设置块属性值，blkref 为块参照的图元名，lst 为多个属性名和属性值组成的点对表。"
  ""
  "(block:set-attributes blkref '((\"att1\"
        . \"value1\")(\"att2\"
        . \"value2\")))"
  (if (= (quote ename)
      (type blkref))
    (setq blkref (e2o blkref)))
  (if (= (quote vla-object)
      (type blkref))
    (if (safearray-value (setq atts (vlax-variant-value (vla-getattributes blkref))))
      (progn (foreach n lst (mapcar (quote (lambda (x)
                (if (= (strcase (car n))
                    (strcase (vla-get-tagstring x)))
                  (vla-put-textstring x (cdr n)))))
            (vlax-safearray->list atts)))
        (vla-update blkref)))
    nil))
```
</details>

---

### block:set-dynprop

**说明**: 设置动态块特性值

**用法**:
```lisp
(block:set-dynprop blkref prp val)
```

**参数**: 1 blkref  : 块图元/对象;2 prp  : 块特性;3 val  : 值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:set-dynprop (blkref prp val)
  "设置动态块特性值"
  (setq prp (strcase prp))
  (vl-some (quote (lambda (x)
        (if (= prp (strcase (vla-get-propertyname x)))
          (progn (vla-put-value x (vlax-make-variant val (vlax-variant-type (vla-get-value x))))
            (cond (val)
              (t))))))
    (vlax-invoke (vlax-ename->vla-object blkref)
      (quote getdynamicblockproperties))))
```
</details>

---

### block:ssget

**说明**: 选择满足指定属性标记及对应值的块。参数：sel-method 参照ssget。当为x时选择全部，无需交互。如需相应的点参数时，需以列表的形式给出如'("c" pt1 pt2)，需手动选择时设为nil。参数: blknames 块名(支持通配符)，或块名列表,或nil参数: lst-attr 属性名与值的点对表或nil

**用法**:
```lisp
(block:ssget sel-method blknames lst-attr)
```

**参数**: 1 sel-method  : 选择集;2 blknames  : 块名称;3 lst-attr  : 列表;

**返回值**: 满足条件的选择集

**示例**:
```lisp
(block:ssget "x" '("块1" "块2")'(("属性1" . "值1")("属性2" . "值2")))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:ssget (sel-method blknames lst-attr / ss ss-res ss0)
  "选择满足指定属性标记及对应值的块。\n参数：sel-method 参照ssget。当为x时选择全部，无需交互。如需相应的点参数时，需以列表的形式给出如'(\"c\"
    pt1 pt2)，需手动选择时设为nil。\n参数: blknames 块名(支持通配符)，或块名列表,或nil\n参数: lst-attr 属性名与值的点对表或nil"
  "满足条件的选择集"
  "(block:ssget \"x\"
    '(\"块1\"
      \"块2\")'((\"属性1\"
        . \"值1\")(\"属性2\"
        . \"值2\")))"
  (cond ((= (quote str)
        (type blknames))
      (setq blknames (list blknames))))
  (setq ss-res (ssadd))
  (cond ((null sel-method)
      (setq ss0 (ssget (quote ((0 . "insert"))))))
    ((p:stringp sel-method)
      (setq ss0 (ssget sel-method (quote ((0 . "insert"))))))
    ((and sel-method (listp sel-method))
      (setq ss0 (apply (quote ssget)
          (append sel-method (list (quote ((0 . "insert"))))))))
    (t (setq ss0 (ssget "x"
          (quote ((0 . "insert")))))))
  (if (setq lst-ent (pickset:to-list ss0))
    (foreach ent% lst-ent (if (and (or (null blknames)
            (member (block:get-effectivename ent%)
              blknames)
            (apply (quote or)
              (mapcar (quote (lambda (x)
                    (wcmatch (block:get-effectivename ent%)
                      x)))
                blknames)))
          (or (null lst-attr)
            (apply (quote and)
              (mapcar (quote (lambda (x)
                    (member x (block:get-attributes ent%))))
                lst-attr))))
        (ssadd ent% ss-res))))
  (sssetfirst nil ss-res)
  ss-res)
```
</details>

---

### block:wcs2bcs

**说明**: 将相对于块引用插入点的世界坐标转为块定义内的坐标

**用法**:
```lisp
(block:wcs2bcs pt pt-base pt-ins ang scale)
```

**参数**: 1 pt  : 单个2D/3D坐标点;2 pt-base  : 单个2D/3D坐标点;3 pt-ins  : 单个2D/3D坐标点;4 ang  : 角度值;5 scale  : 比例值;

**返回值**: 坐标值

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun block:wcs2bcs (pt pt-base pt-ins ang scale / v1)
  "将相对于块引用插入点的世界坐标转为块定义内的坐标"
  "坐标值"
  (setq v1 (mapcar (quote -)
      pt pt-ins))
  (m:coordinate (mapcar (quote *)
      pt-base (quote (1 1 1)))
    (m:coordinate-scale (m:coordinate-rotate v1 (- (* 2 pi)
          ang))
      (/ 1.0 scale))))
```
</details>

---

## cl

**模块说明**: 专用功能模块

### cl:acons

**说明**: 向 alist 前添加键值对。

**用法**:
```lisp
(cl:acons key value alist)
```

**参数**: 1 key  : 键，关键字;2 value  : 值;3 alist  : 未识别定义;

**返回值**: alist

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun cl:acons (key value alist)
  "向 alist 前添加键值对。"
  "alist"
  (cons (cons key value)
    alist))
```
</details>

---

### cl:format

**说明**: common lisp 中 功能强大的格式化输出字符串函数。

**用法**:
```lisp
(cl:format stream ctrl-string variables)
```

**参数**: 1 stream  : 字符串;2 ctrl-string  : 未识别定义;3 variables  : 未识别定义;

**返回值**: String

**示例**:
```lisp
(cl:format "~{~a ~}" (list '("a""b""c")))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun cl:format (stream ctrl-string variables / flag-instruct flag-comma number1 number2 result to-string tmp-str init-flag)
  "common lisp 中 功能强大的格式化输出字符串函数。"
  "String"
  "(cl:format \"~{~a ~}\"
    (list '(\"a\"\"b\"\"c\")))"
  (defun to-string (para)
    (cond ((= (quote int)
          (type-of para))
        (itoa para))
      ((= (quote real)
          (type-of para))
        (rtos para 2 3))
      ((= (quote str)
          (type-of para))
        para)
      ((= (quote list)
          (type-of para))
        (vl-prin1-to-string para))
      ((= (quote sym)
          (type-of para))
        (vl-symbol-name para))))
  (defun init-flag nil (setq flag-instruct nil flag-comma nil number1 ""
      number2 ""))
  (init-flag)
  (setq result "")
  (while (/= ""
      ctrl-string)
    (if flag-instruct (cond ((= (ascii ",")
            (ascii ctrl-string))
          (setq flag-comma t))
        ((= (ascii "v")
            (ascii ctrl-string))
          (setq number1 (to-string (car variables)))
          (setq variables (cdr variables)))
        ((= (ascii "#")
            (ascii ctrl-string))
          (setq number1 (to-string (length variables)))
          (setq variables (cdr variables)))
        ((and (> (ascii ctrl-string)
              47)
            (> 58 (ascii ctrl-string)))
          (if flag-comma (setq number2 (strcat number2 (substr ctrl-string 1 1)))
            (setq number1 (strcat number1 (substr ctrl-string 1 1)))))
        ((= (ascii "~")
            (ascii ctrl-string))
          (setq result (strcat result "~"))
          (init-flag))
        ((= (ascii "%")
            (ascii ctrl-string))
          (setq result (strcat result "\n"))
          (init-flag))
        ((= (ascii "&")
            (ascii ctrl-string))
          (setq result (strcat result "\n"))
          (init-flag))
        ((= (ascii "A")
            (ascii (strcase (substr ctrl-string 1 1))))
          (setq result (strcat result (to-string (car variables))))
          (setq variables (cdr variables))
          (init-flag))
        ((= (ascii "S")
            (ascii (strcase (substr ctrl-string 1 1))))
          (setq result (strcat result (vl-prin1-to-string (car variables))))
          (setq variables (cdr variables))
          (init-flag))
        ((= (ascii "D")
            (ascii (strcase (substr ctrl-string 1 1))))
          (if (/= ""
              number1)
            (progn (setq tmp-str (to-string (car variables)))
              (if (> (atoi number1)
                  (strlen tmp-str))
                (repeat (- (atoi number1)
                    (strlen tmp-str))
                  (setq result (strcat result "
                      "))))
              (setq tmp-str "")))
          (setq result (strcat result (to-string (fix (car variables)))))
          (setq variables (cdr variables))
          (init-flag))
        ((= (ascii "B")
            (ascii (strcase (substr ctrl-string 1 1))))
          (if (/= ""
              number1)
            (progn (setq tmp-str (to-string (car variables)))
              (if (> (atoi number1)
                  (strlen tmp-str))
                (repeat (- (atoi number1)
                    (strlen tmp-str))
                  (setq result (strcat result "
                      "))))
              (setq tmp-str "")))
          (setq result (strcat result (m:dec->base (fix (car variables))
                2)))
          (setq variables (cdr variables))
          (init-flag))
        ((= (ascii "O")
            (ascii (strcase (substr ctrl-string 1 1))))
          (if (/= ""
              number1)
            (progn (setq tmp-str (to-string (car variables)))
              (if (> (atoi number1)
                  (strlen tmp-str))
                (repeat (- (atoi number1)
                    (strlen tmp-str))
                  (setq result (strcat result "
                      "))))
              (setq tmp-str "")))
          (setq result (strcat result (m:dec->base (fix (car variables))
                8)))
          (setq variables (cdr variables))
          (init-flag))
        ((= (ascii "X")
            (ascii (strcase (substr ctrl-string 1 1))))
          (if (/= ""
              number1)
            (progn (setq tmp-str (to-string (car variables)))
              (if (> (atoi number1)
                  (strlen tmp-str))
                (repeat (- (atoi number1)
                    (strlen tmp-str))
                  (setq result (strcat result "
                      "))))
              (setq tmp-str "")))
          (setq result (strcat result (m:dec->base (fix (car variables))
                16)))
          (setq variables (cdr variables))
          (init-flag))
        ((= (ascii "F")
            (ascii (strcase (substr ctrl-string 1 1))))
          (if (= ""
              number2)
            (setq tmp-str (rtos (car variables)
                2 3))
            (setq tmp-str (rtos (car variables)
                2 (atoi number2))))
          (if (/= ""
              number1)
            (progn (if (> (atoi number1)
                  (strlen tmp-str))
                (repeat (- (atoi number1)
                    (strlen tmp-str))
                  (setq result (strcat result "
                      "))))))
          (setq result (strcat result tmp-str))
          (setq tmp-str "")
          (setq variables (cdr variables))
          (init-flag))
        ((= (ascii "E")
            (ascii (strcase (substr ctrl-string 1 1))))
          (if (= ""
              number2)
            (setq tmp-str (rtos (car variables)
                1 3))
            (setq tmp-str (rtos (car variables)
                1 (atoi number2))))
          (if (/= ""
              number1)
            (progn (if (> (atoi number1)
                  (strlen tmp-str))
                (repeat (- (atoi number1)
                    (strlen tmp-str))
                  (setq result (strcat result "
                      "))))))
          (setq result (strcat result tmp-str))
          (setq tmp-str "")
          (setq variables (cdr variables))
          (init-flag))
        ((= (ascii "$")
            (ascii (strcase (substr ctrl-string 1 1))))
          (if (= ""
              number1)
            (setq result (strcat result (rtos (car variables)
                  2 2)))
            (setq result (strcat result (rtos (car variables)
                  2 (atoi number1)))))
          (setq variables (cdr variables))
          (init-flag))
        ((= (ascii "{")
            (ascii (strcase (substr ctrl-string 1 1))))
          (setq ctrl-string (substr ctrl-string 2))
          (setq sub-ctrl-string "")
          (while (and (<= 2 (strlen ctrl-string))
              (/= "~}"
                (substr ctrl-string 1 2)))
            (setq sub-ctrl-string (strcat sub-ctrl-string (substr ctrl-string 1 1)))
            (setq ctrl-string (substr ctrl-string 2)))
          (setq ctrl-string (substr ctrl-string 3))
          (foreach para (car variables)
            (if (atom para)
              (setq para (list para)))
            (setq result (strcat result (cl:format nil sub-ctrl-string para))))
          (setq variables (cdr variables))
          (init-flag))
        ((= (ascii "[")
            (ascii (strcase (substr ctrl-string 1 1))))
          (setq ctrl-string (substr ctrl-string 2))
          (setq sub-ctrl-string "")
          (while (and (<= 2 (strlen ctrl-string))
              (/= "~]"
                (substr ctrl-string 1 2)))
            (setq sub-ctrl-string (strcat sub-ctrl-string (substr ctrl-string 1 1)))
            (setq ctrl-string (substr ctrl-string 2)))
          (setq ctrl-string (substr ctrl-string 3))
          (setq sub-lst (string:parse-by-lst sub-ctrl-string (quote ("~;"
                  "~:;"))))
          (cond ((< (car variables)
                (length sub-lst))
              (setq result (strcat result (nth (car variables)
                    sub-lst))))
            (t (if (vl-string-search "`~:;"
                  sub-ctrl-string)
                (setq result (strcat result (last sub-lst))))))
          (setq variables (cdr variables))
          (init-flag)))
      (cond ((= (ascii "~")
            (ascii ctrl-string))
          (setq flag-instruct t))
        (t (setq result (strcat result (substr ctrl-string 1 1))))))
    (setq ctrl-string (substr ctrl-string 2)))
  (cond ((= t stream)
      (princ result)
      (princ))
    ((= nil stream)
      result)
    ((= (type stream)
        (quote file))
      (write-line result stream))))
```
</details>

---

### cl:handle-keywords

**说明**: 处理lisp表达式或form中的 关键字符号，因autolisp的eval不支持。在eval前进行处理。

**用法**:
```lisp
(cl:handle-keywords expr)
```

**参数**: 1 expr  : 表达式，一般指lisp表达式;

**返回值**: expr

**示例**:
```lisp
(cl:handle-keywords '(:a 1 :b 2 :c 3 :d "abc"))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun cl:handle-keywords (expr)
  "处理lisp表达式或form中的 关键字符号，因autolisp的eval不支持。在eval前进行处理。"
  "expr"
  "(cl:handle-keywords '(:a 1 :b 2 :c 3 :d \"abc\"))"
  (cond ((listp expr)
      (mapcar (quote handle-keywords)
        expr))
    ((= (type expr)
        (quote sym))
      (if (and (= (ascii ":")
            (ascii (vl-symbol-name expr)))
          (null expr))
        (progn (setq sym-str (vl-symbol-name expr))
          (set (read sym-str)
            (read sym-str)))
        (read (vl-symbol-name expr))))
    (t expr)))
```
</details>

---

### cl:let

**说明**: commonlisp中的let，在autolisp中需要在参数前加quote，当body为多条语句时需要用 progn 包裹。

**用法**:
```lisp
(cl:let bindings body)
```

**参数**: 1 bindings  : 未识别定义;2 body  : 未识别定义;

**返回值**: any

**示例**:
```lisp
(let '((a 3) (b 5)) '(* a b)) => 15
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun cl:let (bindings body)
  "commonlisp中的let，在autolisp中需要在参数前加quote，当body为多条语句时需要用 progn 包裹。"
  "any"
  "(let '((a 3)
      (b 5))
    '(* a b))
  => 15"
  (eval (cons (list (quote lambda)
        (mapcar (quote car)
          bindings)
        body)
      (mapcar (quote cadr)
        bindings))))
```
</details>

---

### cl:pairlis

**说明**: 两个列表构成 alisp

**用法**:
```lisp
(cl:pairlis keys values)
```

**参数**: 1 keys  : 键，关键字;2 values  : 值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun cl:pairlis (keys values)
  "两个列表构成 alisp"
  (mapcar (quote (lambda (x y)
        (cons x y)))
    keys values))
```
</details>

---

### cl:rassoc

**说明**: 反向查询 alist,即使用每个元素的CDR中的值作为 key

**用法**:
```lisp
(cl:rassoc key alist)
```

**参数**: 1 key  : 键，关键字;2 alist  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun cl:rassoc (key alist / x)
  "反向查询 alist,即使用每个元素的CDR中的值作为 key"
  (setq x (assoc key (mapcar (quote (lambda (x)
            (cons (cdr x)
              (car x))))
        alist)))
  (if x (cons (cdr x)
      (car x))))
```
</details>

---

## clipboard

**模块说明**: 专用功能模块

### clipboard:cleardata

**说明**: 清空剪贴板内容

**用法**:
```lisp
(clipboard:cleardata )
```

**参数**: None

**返回值**: -1

**示例**:
```lisp
(clipboard:cleardata)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun clipboard:cleardata (/ cb)
  "清空剪贴板内容"
  "-1"
  "(clipboard:cleardata)"
  (clipboard:init)
  (vlax-invoke @:*clipboard* (quote cleardata)
    "text"))
```
</details>

---

### clipboard:getdata

**说明**: 获取剪贴板内容

**用法**:
```lisp
(clipboard:getdata )
```

**参数**: None

**返回值**: string or nil

**示例**:
```lisp
(clipboard:getdata)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun clipboard:getdata (/ cb)
  "获取剪贴板内容"
  "string or nil"
  "(clipboard:getdata)"
  (clipboard:init)
  (vlax-invoke @:*clipboard* (quote getdata)
    "TEXT"))
```
</details>

---

### clipboard:init

**说明**: 初始化剪贴板对象。

**用法**:
```lisp
(clipboard:init )
```

**参数**: 没有一个

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun clipboard:init nil "初始化剪贴板对象。"
  ""
  (setq @:*clipboard* (vlax-get (vlax-get-property (vlax-get-or-create-object "HTMLFILE")
        (quote parentwindow))
      (quote clipboarddata))))
```
</details>

---

### clipboard:setdata

**说明**: 设置剪贴板内容为 str.

**用法**:
```lisp
(clipboard:setdata str)
```

**参数**: 1 str  : 字符串;

**返回值**: -1

**示例**:
```lisp
(clipboard:setdata "the string in clipboard.")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun clipboard:setdata (str / cb)
  "设置剪贴板内容为 str."
  "-1"
  "(clipboard:setdata \"the string in clipboard.\")"
  (clipboard:init)
  (vlax-invoke @:*clipboard* (quote setdata)
    "text"
    str))
```
</details>

---

## color

**模块说明**: 颜色管理、格式转换

### color:aci2rgb

**说明**: 索引色转rgb, num 范围 1 - 255

**用法**:
```lisp
(color:aci2rgb num)
```

**参数**: 1 num  : 数值;

**返回值**: lst, 红绿蓝三色值

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun color:aci2rgb (num)
  "索引色转rgb, num 范围 1 - 255"
  "lst, 红绿蓝三色值"
  (if (numberp num)
    (setq num (fix num))
    (setq num 0))
  (if (< 0 num 256)
    (progn (setq aci (quote ((1 255 0 0)
            (2 255 255 0)
            (3 0 255 0)
            (4 0 255 255)
            (5 0 0 255)
            (6 255 0 255)
            (7 255 255 255)
            (8 128 128 128)
            (9 192 192 192)
            (10 255 0 0)
            (11 255 127 127)
            (12 204 0 0)
            (13 204 102 102)
            (14 153 0 0)
            (15 153 76 76)
            (16 127 0 0)
            (17 127 63 63)
            (18 76 0 0)
            (19 76 38 38)
            (20 255 63 0)
            (21 255 159 127)
            (22 204 51 0)
            (23 204 127 102)
            (24 153 38 0)
            (25 153 95 76)
            (26 127 31 0)
            (27 127 79 63)
            (28 76 19 0)
            (29 76 47 38)
            (30 255 127 0)
            (31 255 191 127)
            (32 204 102 0)
            (33 204 153 102)
            (34 153 76 0)
            (35 153 114 76)
            (36 127 63 0)
            (37 127 95 63)
            (38 76 38 0)
            (39 76 57 38)
            (40 255 191 0)
            (41 255 223 127)
            (42 204 153 0)
            (43 204 178 102)
            (44 153 114 0)
            (45 153 133 76)
            (46 127 95 0)
            (47 127 111 63)
            (48 76 57 0)
            (49 76 66 38)
            (50 255 255 0)
            (51 255 255 127)
            (52 204 204 0)
            (53 204 204 102)
            (54 153 153 0)
            (55 153 153 76)
            (56 127 127 0)
            (57 127 127 63)
            (58 76 76 0)
            (59 76 76 38)
            (60 191 255 0)
            (61 223 255 127)
            (62 153 204 0)
            (63 178 204 102)
            (64 114 153 0)
            (65 133 153 76)
            (66 95 127 0)
            (67 111 127 63)
            (68 57 76 0)
            (69 66 76 38)
            (70 127 255 0)
            (71 191 255 127)
            (72 102 204 0)
            (73 153 204 102)
            (74 76 153 0)
            (75 114 153 76)
            (76 63 127 0)
            (77 95 127 63)
            (78 38 76 0)
            (79 57 76 38)
            (80 63 255 0)
            (81 159 255 127)
            (82 51 204 0)
            (83 127 204 102)
            (84 38 153 0)
            (85 95 153 76)
            (86 31 127 0)
            (87 79 127 63)
            (88 19 76 0)
            (89 47 76 38)
            (90 0 255 0)
            (91 127 255 127)
            (92 0 204 0)
            (93 102 204 102)
            (94 0 153 0)
            (95 76 153 76)
            (96 0 127 0)
            (97 63 127 63)
            (98 0 76 0)
            (99 38 76 38)
            (100 0 255 63)
            (101 127 255 159)
            (102 0 204 51)
            (103 102 204 127)
            (104 0 153 38)
            (105 76 153 95)
            (106 0 127 31)
            (107 63 127 79)
            (108 0 76 19)
            (109 38 76 47)
            (110 0 255 127)
            (111 127 255 191)
            (112 0 204 102)
            (113 102 204 153)
            (114 0 153 76)
            (115 76 153 114)
            (116 0 127 63)
            (117 63 127 95)
            (118 0 76 38)
            (119 38 76 57)
            (120 0 255 191)
            (121 127 255 223)
            (122 0 204 153)
            (123 102 204 178)
            (124 0 153 114)
            (125 76 153 133)
            (126 0 127 95)
            (127 63 127 111)
            (128 0 76 57)
            (129 38 76 66)
            (130 0 255 255)
            (131 127 255 255)
            (132 0 204 204)
            (133 102 204 204)
            (134 0 153 153)
            (135 76 153 153)
            (136 0 127 127)
            (137 63 127 127)
            (138 0 76 76)
            (139 38 76 76)
            (140 0 191 255)
            (141 127 223 255)
            (142 0 153 204)
            (143 102 178 204)
            (144 0 114 153)
            (145 76 133 153)
            (146 0 95 127)
            (147 63 111 127)
            (148 0 57 76)
            (149 38 66 76)
            (150 0 127 255)
            (151 127 191 255)
            (152 0 102 204)
            (153 102 153 204)
            (154 0 76 153)
            (155 76 114 153)
            (156 0 63 127)
            (157 63 95 127)
            (158 0 38 76)
            (159 38 57 76)
            (160 0 63 255)
            (161 127 159 255)
            (162 0 51 204)
            (163 102 127 204)
            (164 0 38 153)
            (165 76 95 153)
            (166 0 31 127)
            (167 63 79 127)
            (168 0 19 76)
            (169 38 47 76)
            (170 0 0 255)
            (171 127 127 255)
            (172 0 0 204)
            (173 102 102 204)
            (174 0 0 153)
            (175 76 76 153)
            (176 0 0 127)
            (177 63 63 127)
            (178 0 0 76)
            (179 38 38 76)
            (180 63 0 255)
            (181 159 127 255)
            (182 51 0 204)
            (183 127 102 204)
            (184 38 0 153)
            (185 95 76 153)
            (186 31 0 127)
            (187 79 63 127)
            (188 19 0 76)
            (189 47 38 76)
            (190 127 0 255)
            (191 191 127 255)
            (192 102 0 204)
            (193 153 102 204)
            (194 76 0 153)
            (195 114 76 153)
            (196 63 0 127)
            (197 95 63 127)
            (198 38 0 76)
            (199 57 38 76)
            (200 191 0 255)
            (201 223 127 255)
            (202 153 0 204)
            (203 178 102 204)
            (204 114 0 153)
            (205 133 76 153)
            (206 95 0 127)
            (207 111 63 127)
            (208 57 0 76)
            (209 66 38 76)
            (210 255 0 255)
            (211 255 127 255)
            (212 204 0 204)
            (213 204 102 204)
            (214 153 0 153)
            (215 153 76 153)
            (216 127 0 127)
            (217 127 63 127)
            (218 76 0 76)
            (219 76 38 76)
            (220 255 0 191)
            (221 255 127 223)
            (222 204 0 153)
            (223 204 102 178)
            (224 153 0 114)
            (225 153 76 133)
            (226 127 0 95)
            (227 127 63 111)
            (228 76 0 57)
            (229 76 38 66)
            (230 255 0 127)
            (231 255 127 191)
            (232 204 0 102)
            (233 204 102 153)
            (234 153 0 76)
            (235 153 76 114)
            (236 127 0 63)
            (237 127 63 95)
            (238 76 0 38)
            (239 76 38 57)
            (240 255 0 63)
            (241 255 127 159)
            (242 204 0 51)
            (243 204 102 127)
            (244 153 0 38)
            (245 153 76 95)
            (246 127 0 31)
            (247 127 63 79)
            (248 76 0 19)
            (249 76 38 47)
            (250 51 51 51)
            (251 91 91 91)
            (252 132 132 132)
            (253 173 173 173)
            (254 214 214 214)
            (255 255 255 255))))
      (cdr (assoc num aci)))
    (quote (0 0 0))))
```
</details>

---

### color:bookcolor2rgb

**说明**: 将配色系统颜色名转为rgb。

**用法**:
```lisp
(color:bookcolor2rgb bookcolor)
```

**参数**: 1 bookcolor  : 未识别定义;

**返回值**: list

**示例**:
```lisp
(color:bookcolor2rgb "DIC COLOR GUIDE(R)$DIC 4")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun color:bookcolor2rgb (bookcolor / bc ci)
  "将配色系统颜色名转为rgb。"
  "list"
  "(color:bookcolor2rgb \"DIC COLOR GUIDE(R)$DIC 4\")"
  (setq bc (string:to-list bookcolor "$"))
  (vla-setcolorbookcolor (setq ci (color:interface))
    (car bc)
    (cadr bc))
  (list (vla-get-red ci)
    (vla-get-green ci)
    (vla-get-blue ci)))
```
</details>

---

### color:interface

**说明**: CAD 颜色接口对象

**用法**:
```lisp
(color:interface )
```

**参数**: 没有一个

**返回值**: ActiveX

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun color:interface nil "CAD 颜色接口对象"
  "ActiveX"
  (vlax-create-object (strcat "AutoCAD.AcCmColor."
      (substr (getvar "ACADVER")
        1 2))))
```
</details>

---

### color:rgb

**说明**: 计算rgb颜色对应的整数值。red green blue 取值范围为 [0,255]的整数或[0,1)的小数。

**用法**:
```lisp
(color:rgb red green blue)
```

**参数**: 1 red  : 未识别定义;2 green  : 未识别定义;3 blue  : 未识别定义;

**返回值**: rgb颜色值

**示例**:
```lisp
(color:rgb 255 0 0) or (color:rgb 0.999 0 0);红色
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun color:rgb (red green blue)
  "计算rgb颜色对应的整数值。red green blue 取值范围为 [0,255]的整数或[0,1)的小数。"
"rgb颜色值"
"(color:rgb 255 0 0)
or (color:rgb 0.999 0 0);红色"
(cond ((and (<= 0 red)
      (< red 1)
      (<= 0 green)
      (< green 1)
      (<= 0 blue)
      (< blue 1))
    (+ (fix (* 256 red))
      (* 256 (fix (* 256 green)))
      (* 65536 (fix (* 256 blue)))))
  ((and (<= 0 red 255)
      (<= 0 green 255)
      (<= 0 blue 255))
    (+ (fix red)
      (* 256 (fix green))
      (* 65536 (fix blue))))
  (t (+ 255 (* 256 255)
      (* 65536 255)))))
```
</details>

---

### color:rgb2css

**说明**: 将 color 的三色值转换为 #FFFFFF 样式的字符串

**用法**:
```lisp
(color:rgb2css lst-color)
```

**参数**: 1 lst-color  : 列表;

**返回值**: String

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun color:rgb2css (lst-color)
  "将 color 的三色值转换为 #FFFFFF 样式的字符串"
  "String"
  (strcat "#"
    (string:from-list (mapcar (quote (lambda (x / c)
            (setq c (substr (vl-symbol-name (m:dec2hex x))
                3))
            (if (= 1 (strlen c))
              (strcat "0"
                c)
              c)))
        lst-color)
      "")))
```
</details>

---

### color:rgb2truecolor

**说明**: RGB值列表转为真彩色值

**用法**:
```lisp
(color:rgb2truecolor rgb)
```

**参数**: 1 rgb  : 未识别定义;

**返回值**: long

**示例**:
```lisp
(color:rgb2truecolor (list 3 5 8))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun color:rgb2truecolor (rgb / ci)
  "RGB值列表转为真彩色值"
  "long"
  "(color:rgb2truecolor (list 3 5 8))"
  (setq ci (color:interface))
  (vla-put-red ci (car rgb))
  (vla-put-green (cadr ci))
  (vla-put-blue (caddr ci))
  (vla-get-entitycolor ci))
```
</details>

---

### color:truecolor2aci

**说明**: 真彩色号转为近似的索引色号

**用法**:
```lisp
(color:truecolor2aci long)
```

**参数**: 1 long  : 未识别定义;

**返回值**: int

**示例**:
```lisp
(color:truecolor2aci 2076128) => 142
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun color:truecolor2aci (long / ci)
  "真彩色号转为近似的索引色号"
  "int"
  "(color:truecolor2aci 2076128)
  => 142"
  (setq ci (color:interface))
  (vla-put-entitycolor ci long)
  (vla-get-colorindex ci))
```
</details>

---

### color:truecolor2rgb

**说明**: 真彩色号转为RGB值列表

**用法**:
```lisp
(color:truecolor2rgb long)
```

**参数**: 1 long  : 未识别定义;

**返回值**: list

**示例**:
```lisp
(color:truecolor2rgb 2076128)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun color:truecolor2rgb (long / ci)
  "真彩色号转为RGB值列表"
  "list"
  "(color:truecolor2rgb 2076128)"
  (setq ci (color:interface))
  (vla-put-entitycolor ci long)
  (list (vla-get-red ci)
    (vla-get-green ci)
    (vla-get-blue ci)))
```
</details>

---

## crypto

**模块说明**: 专用功能模块

### crypto:md5

**说明**: 对字符串str进行md5散列，注意：对于宽字节文字，其编码为unicode，而不是utf8或ansi

**用法**:
```lisp
(crypto:md5 str)
```

**参数**: 1 str  : 字符串;

**返回值**: str

**示例**:
```lisp
(crypto:md5 "abc") ;;=> 900150983cd24fb0d6963f7d28e17f72
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun crypto:md5 (str)
  "对字符串str进行md5散列，注意：对于宽字节文字，其编码为unicode，而不是utf8或ansi"
  "str"
  "(crypto:md5 \"abc\")
  ;;=> 900150983cd24fb0d6963f7d28e17f72"
  (@:post (strcat (@:uri)
      "/crypto.php?method=md5")
    (strcat "q="
      str)))
```
</details>

---

## curve

**模块说明**: 曲线处理、长度计算、优化

### curve:3pt+chord2pt

**说明**: 已知圆弧上三点pt1,pt2,pt3,求此圆弧上和pt-on-arc弦长为chord-length的点

**用法**:
```lisp
(curve:3pt+chord2pt pt1 pt2 pt3 chord-length pt-on-arc)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;2 pt2  : 单个2D/3D坐标点;3 pt3  : 单个2D/3D坐标点;4 chord-length  : 未识别定义;5 pt-on-arc  : 单个2D/3D坐标点;

**返回值**: pt

**示例**:
```lisp
(curve:3pt+chord2pt pt1 pt2 pt3 chord-length pt-on-arc)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:3pt+chord2pt (pt1 pt2 pt3 chord-length pt-on-arc / angle1 angle2 center half-chord-angle pt-other radius)
  "已知圆弧上三点pt1,pt2,pt3,求此圆弧上和pt-on-arc弦长为chord-length的点"
  "pt"
  "(curve:3pt+chord2pt pt1 pt2 pt3 chord-length pt-on-arc)"
  (setq center (curve:3pt2o pt1 pt2 pt3)
    radius (distance center pt1))
  (setq half-chord-angle (m:acos (- 1 (/ (expt chord-length 2)
          (* 2 (expt radius 2))))))
  (setq angle1 (angle center pt-on-arc))
  (if (> (geometry:turn-right-p pt1 pt2 pt3)
      0)
    (setq angle2 (+ angle1 half-chord-angle))
    (setq angle2 (- angle1 half-chord-angle)))
  (setq pt-other (polar center angle2 radius))
  pt-other)
```
</details>

---

### curve:3pt2bulge

**说明**: 三点求凸度，任意两点的垂线的交点即圆心。

**用法**:
```lisp
(curve:3pt2bulge pt1 pt2 pt3)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;2 pt2  : 单个2D/3D坐标点;3 pt3  : 单个2D/3D坐标点;

**返回值**: number

**示例**:
```lisp
(curve:3pt2bulge (getpoint)(getpoint)(getpoint))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:3pt2bulge (pt1 pt2 pt3 / ptm1 ptm2)
  "三点求凸度，任意两点的垂线的交点即圆心。"
  "number"
  "(curve:3pt2bulge (getpoint)(getpoint)(getpoint))"
  (* (if (> (geometry:turn-right-p pt1 pt2 pt3)
        0)
      1 -1)
    ((if (> (geometry:turn-right-p pt1 pt2 pt3)
          0)
        curve:o2bulge curve:o2bulge-clockwise)
      pt1 pt3 (curve:3pt2o pt1 pt2 pt3))))
```
</details>

---

### curve:3pt2bulge-plus

**说明**: 三点求凸度，任意两点的垂线的交点即圆心。增加pt2是否在圆弧上的判断

**用法**:
```lisp
(curve:3pt2bulge-plus pt1 pt2 pt3)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;2 pt2  : 单个2D/3D坐标点;3 pt3  : 单个2D/3D坐标点;

**返回值**: number

**示例**:
```lisp
(curve:3pt2bulge-plus (getpoint)(getpoint)(getpoint))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:3pt2bulge-plus (pt1 pt2 pt3)
  "三点求凸度，任意两点的垂线的交点即圆心。增加pt2是否在圆弧上的判断"
  "number"
  "(curve:3pt2bulge-plus (getpoint)(getpoint)(getpoint))"
  (if (curve:pt-in-arc-p pt2 pt1 pt3 (curve:3pt2bulge pt1 pt2 pt3))
    (curve:3pt2bulge pt1 pt2 pt3)
    (curve:3pt2bulge-clockwise pt1 pt2 pt3)))
```
</details>

---

### curve:3pt2o

**说明**: 三点求圆心，任意两点的垂线的交点即圆心。

**用法**:
```lisp
(curve:3pt2o pt1 pt2 pt3)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;2 pt2  : 单个2D/3D坐标点;3 pt3  : 单个2D/3D坐标点;

**返回值**: number

**示例**:
```lisp
(curve:3pt2o (getpoint)(getpoint)(getpoint))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:3pt2o (pt1 pt2 pt3 / ptm1 ptm2)
  "三点求圆心，任意两点的垂线的交点即圆心。"
  "number"
  "(curve:3pt2o (getpoint)(getpoint)(getpoint))"
  (inters (setq ptm1 (point:mid pt1 pt2))
    (polar ptm1 (m:fix-angle (+ (* 0.5 pi)
          (angle pt1 pt2)))
      1000)
    (setq ptm2 (point:mid pt1 pt3))
    (polar ptm2 (m:fix-angle (+ (* 0.5 pi)
          (angle pt1 pt3)))
      1000)
    nil))
```
</details>

---

### curve:arc2lwpl

**说明**: 将圆弧转换成 由 int 段组成的多段线, int 不小于 1

**用法**:
```lisp
(curve:arc2lwpl ent-arc int)
```

**参数**: 1 ent-arc  : 单个图元;2 int  : 整数;

**返回值**: 多段线图元

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:arc2lwpl (ent-arc int / pt-center r pts bulge convexity ent i)
  "将圆弧转换成 由 int 段组成的多段线, int 不小于 1"
  "多段线图元"
  (setq int (fix int))
  (if (< (fix int)
      1)
    (setq int 1))
  (setq pt-center (entity:getdxf ent-arc 10)
    r (entity:getdxf ent-arc 40))
  (setq pts (quote nil))
  (setq begin-ang (entity:getdxf ent-arc 50))
  (setq end-ang (entity:getdxf ent-arc 51))
  (setq rad (- (entity:getdxf ent-arc 51)
      (entity:getdxf ent-arc 50)))
  (if (< rad 0)
    (setq rad (+ rad (* 2 pi))))
  (setq bulge (curve:o2bulge (polar pt-center begin-ang r)
      (polar pt-center (+ begin-ang (/ rad int))
        r)
      pt-center))
  (setq i 0)
  (repeat (1+ int)
    (setq pts (cons (polar pt-center (+ begin-ang (* i (/ rad int)))
          r)
        pts))
    (setq i (1+ i))
    (setq convexity (cons (- bulge)
        convexity)))
  (push-var)
  (setvar "osmode"
    0)
  (setq ent (entity:putdxf (entity:putdxf (entity:make-lwpline-bold pts convexity nil 0 0)
        8 (entity:getdxf ent-arc 8))
      62 (if (entity:getdxf ent-arc 62)
        (entity:getdxf ent-arc 62)
        256)))
  (pop-var)
  ent)
```
</details>

---

### curve:bulge2o

**说明**: 求凸度bulge 和两点 pt1 pt2 表示的弧的圆心。

**用法**:
```lisp
(curve:bulge2o pt1 pt2 bulge)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;2 pt2  : 单个2D/3D坐标点;3 bulge  : 凸度;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:bulge2o (pt1 pt2 bulge / b x1 y1 x2 y2)
  "求凸度bulge 和两点 pt1 pt2 表示的弧的圆心。"
  (setq b (* 0.5 (- (/ 1.0 bulge)
        bulge)))
  (setq x1 (car pt1)
    y1 (cadr pt1)
    x2 (car pt2)
    y2 (cadr pt2)
    z1 (caddr pt1)
    z2 (caddr pt2))
  (if z1 (list (* 0.5 (- (+ x1 x2)
          (* b (- y2 y1))))
      (* 0.5 (+ (+ y1 y2)
          (* b (- x2 x1))))
      z1)
    (list (* 0.5 (- (+ x1 x2)
          (* b (- y2 y1))))
      (* 0.5 (+ (+ y1 y2)
          (* b (- x2 x1)))))))
```
</details>

---

### curve:chain-line

**用法**:
```lisp
(curve:chain-line pt1)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:chain-line (pt1 / segments pts-ent i%)
  (setq lst-ss1 (pickset:to-list (ssget "C"
        (polar pt1 (* 1.25 pi)
          1)
        (polar pt1 (* 0.25 pi)
          1)
        (quote ((0 . "LINE,LWPOLYLINE,POLYLINE"))))))
  (setq pts (quote nil))
  (setq segments (quote nil))
  (setq pts (curve:pline-3dpoints (car lst-ss1)))
  (print pts)
  (defun ssget-by-point (pt1 / lst-ss1)
    (push-var)
    (setvar "osmode"
      0)
    (setq lst-ss1 (pickset:to-list (ssget "C"
          (polar pt1 (* 1.25 pi)
            10)
          (polar pt1 (* 0.25 pi)
            10)
          (quote ((0 . "LINE,LWPOLYLINE,POLYLINE"))))))
    (pop-var)
    lst-ss1)
  (setq i% 0)
  (setq pt-old nil)
  (while (and (> (length (setq lst-ss-pre (ssget-by-point (car pts))))
        1)
      (/= pt-old (car pts))
      (< i% 50))
    (setq pt-old (car pts))
    (entity:make-point (car pts))
    (foreach ent% lst-ss-pre (if (null (member ent% lst-ss1))
        (progn (setq lst-ss1 (cons ent% lst-ss1))
          (setq pts-ent (curve:pline-3dpoints ent%))
          (if (< (distance (car pts-ent)
                (car pts))
              0.001)
            (setq pts (append (reverse pts-ent)
                (cdr pts)))
            (if (< (distance (last pts-ent)
                  (car pts))
                0.001)
              (setq pts (append pts-ent (cdr pts))))))))
    (setq i% (1+ i%)))
  (while (and (> (length (setq lst-ss-pre (ssget-by-point (last pts))))
        1)
      (/= pt-old (last pts))
      (< i% 50))
    (setq pt-old (last pts))
    (entity:make-point (car pts))
    (foreach ent% lst-ss-pre (if (null (member ent% lst-ss1))
        (progn (setq lst-ss1 (append lst-ss1 (list ent%)))
          (setq pts-ent (curve:pline-3dpoints ent%))
          (if (< (distance (car pts-ent)
                (last pts))
              0.001)
            (setq pts (append pts (cdr pts-ent)))
            (if (< (distance (last pts-ent)
                  (last pts))
                0.001)
              (setq pts (append pts (cdr (reverse pts-ent)))))))))
    (setq i% (1+ i%)))
  (setq pl-new (entity:make-lwpline-bold pts nil 0 0 0.3))
  (print pts))
```
</details>

---

### curve:checkarc

**说明**: 判断多段线是否有圆弧(凸度/=0)的子段

**用法**:
```lisp
(curve:checkarc en)
```

**参数**: 1 en  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:checkarc (en / g)
  "判断多段线是否有圆弧(凸度/=0)的子段"
  (setq g (vl-remove-if-not (quote (lambda (x)
          (= (car x)
            42)))
      (entget en)))
  (not (vl-every (quote zerop)
      (mapcar (quote cdr)
        g))))
```
</details>

---

### curve:circle2lwpl

**说明**: 将圆转换成 由 int 段组成的多段线。

**用法**:
```lisp
(curve:circle2lwpl ent-circle int)
```

**参数**: 1 ent-circle  : 单个图元;2 int  : 整数;

**返回值**: 多段线图元

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:circle2lwpl (ent-circle int / pt-center r pts bulge convexity ent i)
  "将圆转换成 由 int 段组成的多段线。"
  "多段线图元"
  (setq int (fix int))
  (if (< (fix int)
      2)
    (setq int 2))
  (setq pt-center (entity:getdxf ent-circle 10)
    r (entity:getdxf ent-circle 40))
  (setq pts (quote nil))
  (setq bulge (curve:o2bulge (polar pt-center 0 r)
      (polar pt-center (/ (* 2 pi)
          int)
        r)
      pt-center))
  (setq i 0)
  (repeat int (setq pts (cons (polar pt-center (* i (/ (* 2 pi)
              int))
          r)
        pts))
    (setq i (1+ i))
    (setq convexity (cons (- bulge)
        convexity)))
  (push-var)
  (setvar "osmode"
    0)
  (setq ent (entity:putdxf (entity:putdxf (entity:make-lwpline-bold pts convexity nil 1 0)
        8 (entity:getdxf ent-circle 8))
      62 (if (entity:getdxf ent-circle 62)
        (entity:getdxf ent-circle 62)
        256)))
  (pop-var)
  ent)
```
</details>

---

### curve:circle2pts

**说明**: 求圆上 int 个均分的点。

**用法**:
```lisp
(curve:circle2pts ent-circle int)
```

**参数**: 1 ent-circle  : 单个图元;2 int  : 整数;

**返回值**: list

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:circle2pts (ent-circle int / pt-center r pts bulge convexity ent i)
  "求圆上 int 个均分的点。"
  "list"
  (setq int (fix int))
  (if (< (fix int)
      2)
    (setq int 2))
  (setq pt-center (entity:getdxf ent-circle 10)
    r (entity:getdxf ent-circle 40))
  (setq pts (quote nil))
  (setq i 0)
  (repeat int (setq pts (cons (polar pt-center (* i (/ (* 2 pi)
              int))
          r)
        pts))
    (setq i (1+ i)))
  pts)
```
</details>

---

### curve:clockwisep

**说明**: 判断多段线方向

**用法**:
```lisp
(curve:clockwisep ent)
```

**参数**: 1 ent  : 单个图元;

**返回值**: 顺时针返回t，反之nil

**示例**:
```lisp
(curve:clockwisep (car(entsel)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:clockwisep (ent / fx offsetobj plineobj)
  "判断多段线方向"
  "顺时针返回t，反之nil"
  "(curve:clockwisep (car(entsel)))"
  (setq plineobj (vlax-ename->vla-object ent))
  (setq offsetplineobj (car (vlax-safearray->list (vlax-variant-value (vla-offset plineobj 0.0001)))))
  (if (> (vlax-curve-getdistatparam plineobj (vlax-curve-getendparam plineobj))
      (vlax-curve-getdistatparam offsetplineobj (vlax-curve-getendparam offsetplineobj)))
    (setq fx t)
    (setq fx nil))
  (vla-delete offsetplineobj)
  fx)
```
</details>

---

### curve:get-angles

**说明**: 取得闭合多段线各点相邻边的夹角角度值

**用法**:
```lisp
(curve:get-angles lwpl)
```

**参数**: 1 lwpl  : 未识别定义;

**返回值**: list

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:get-angles (lwpl / pts angles)
  "取得闭合多段线各点相邻边的夹角角度值"
  "list"
  (setq pts (curve:get-points lwpl))
  (setq pts (append (list (last pts))
      pts (list (car pts))))
  (setq angles (quote nil))
  (while (> (length pts)
      2)
    (setq angles (cons (m:fix-angle (- (angle (cadr pts)
              (caddr pts))
            (angle (car pts)
              (cadr pts))))
        angles))
    (setq pts (cdr pts)))
  (reverse angles))
```
</details>

---

### curve:get-points

**说明**: 曲线控制点及端点列表，返回点坐标。

**用法**:
```lisp
(curve:get-points ent)
```

**参数**: 1 ent  : 单个图元;

**返回值**: 点坐标列表

**示例**:
```lisp
(curve:get-points (car (entsel)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:get-points (ent)
  "曲线控制点及端点列表，返回点坐标。"
  "点坐标列表"
  "(curve:get-points (car (entsel)))"
  (if (p:vlap ent)
    (setq ent (o2e ent)))
  (if (= (quote ename)
      (type ent))
    (cond ((wcmatch (entity:getdxf ent 0)
          "*POLYLINE,LINE,MLINE,CIRCLE,SPLINE,REGION")
        (mapcar (quote cdr)
          (vl-remove-if-not (quote (lambda (x)
                (or (= 10 (car x))
                  (= 11 (car x)))))
            (entget ent))))
      ((wcmatch (entity:getdxf ent 0)
          "ELLIPSE")
        (mapcar (quote cdr)
          (vl-remove-if-not (quote (lambda (x)
                (= 10 (car x))))
            (entget ent))))
      ((wcmatch (entity:getdxf ent 0)
          "ARC")
        (list (polar (entity:getdxf ent 10)
            (entity:getdxf ent 50)
            (entity:getdxf ent 40))
          (polar (entity:getdxf ent 10)
            (entity:getdxf ent 51)
            (entity:getdxf ent 40)))))))
```
</details>

---

### curve:inters

**说明**: 获取对象交点列表参数 obj1 obj2 : 选择集，vla对象，图元名，vla对象表，图元表，nil参数 mode: 该参数只有obj1、obj2为图元或vla对象时，服从下列设置，其他情况均默认对象不延伸     obj1 和 obj2 参数可任意组合，但不能全为nil     acExtendNone 对象不延伸     acExtendThisEntity 延伸obj1     acExtendOtherEntity 延伸obj2     acExtendBoth 对象都延伸     nil = acExtendNone 对象不延伸

**用法**:
```lisp
(curve:inters obj1 obj2 mode)
```

**参数**: 1 obj1  : activeX 对象;2 obj2  : activeX 对象;3 mode  : 未识别定义;

**返回值**: 对象交点列表

**示例**:
```lisp
(curve:inters obj1 obj2 acExtendNone)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:inters (obj1 obj2 mode / getinterpts inter-objlist inter-objlists inter-ss-obj res)
  "获取对象交点列表\n参数 obj1 obj2 : 选择集，vla对象，图元名，vla对象表，图元表，nil\n参数 mode: 该参数只有obj1、obj2为图元或vla对象时，服从下列设置，其他情况均默认对象不延伸\n     obj1 和 obj2 参数可任意组合，但不能全为nil\n     acExtendNone 对象不延伸\n     acExtendThisEntity 延伸obj1\n     acExtendOtherEntity 延伸obj2\n     acExtendBoth 对象都延伸\n     nil = acExtendNone 对象不延伸"
  "对象交点列表"
  "(curve:inters obj1 obj2 acExtendNone)"
  (or mode (setq mode acextendnone))
  (defun getinterpts (obj1 obj2 mode / iplist)
    (or (p:vlap obj1)
      (setq obj1 (e2o obj1)))
    (or (p:vlap obj2)
      (setq obj2 (e2o obj2)))
    (setq iplist (vl-catch-all-apply (quote vlax-safearray->list)
        (list (vlax-variant-value (vla-intersectwith obj1 obj2 mode)))))
    (if (vl-catch-all-error-p iplist)
      nil (list:split-3d iplist)))
  (defun inter-ss-obj (ss obj / res1)
    (cond ((p:picksetp ss)
        (setq ss (pickset:to-vlalist ss)))
      ((p:ename-listp ss)
        (setq ss (mapcar (quote e2o)
            ss))))
    (foreach i ss (setq res1 (append res1 (getinterpts i obj acextendnone))))
    res1)
  (defun inter-objlist (lst / ob1 rtn)
    (cond ((= (quote pickset)
          (type lst))
        (setq lst (pickset:to-vlalist lst)))
      ((p:ename-listp lst)
        (setq lst (mapcar (quote e2o)
            lst))))
    (while (setq ob1 (car lst))
      (foreach ob2 (setq lst (cdr lst))
        (setq rtn (cons (getinterpts ob1 ob2 acextendnone)
            rtn))))
    (apply (quote append)
      (reverse rtn)))
  (defun inter-objlists (ol1 ol2 / rtn)
    (cond ((= (quote pickset)
          (type ol1))
        (setq ol1 (pickset:to-vlalist ol1)))
      ((p:ename-listp ol1)
        (setq ol1 (mapcar (quote e2o)
            ol1))))
    (cond ((= (quote pickset)
          (type ol2))
        (setq ol2 (pickset:to-vlalist ol2)))
      ((p:ename-listp ol1)
        (setq ol2 (mapcar (quote e2o)
            ol2))))
    (foreach ob1 ol1 (foreach ob2 ol2 (setq rtn (cons (getinterpts ob1 ob2 acextendnone)
            rtn))))
    (apply (quote append)
      (reverse rtn)))
  (cond ((and (or (p:vlap obj1)
          (p:enamep obj1))
        (or (p:vlap obj2)
          (p:enamep obj2)))
      (setq res (getinterpts obj1 obj2 mode)))
    ((and (or (p:vlap obj1)
          (p:enamep obj1))
        (or (p:picksetp obj2)
          (p:ename-listp obj2)
          (p:vla-listp obj2)))
      (setq res (inter-ss-obj obj2 obj1)))
    ((and (or (p:vlap obj2)
          (p:enamep obj2))
        (p:picksetp obj1))
      (setq res (inter-ss-obj obj1 obj2)))
    ((and (not obj1)
        (or (p:picksetp obj2)
          (p:ename-listp obj2)
          (p:vla-listp obj2)))
      (setq res (inter-objlist obj2)))
    ((and (not obj2)
        (or (p:picksetp obj1)
          (p:ename-listp obj1)
          (p:vla-listp obj1)))
      (setq res (inter-objlist obj1)))
    ((and (or (p:vla-listp obj1)
          (p:picksetp obj1)
          (p:ename-listp obj1))
        (or (p:vla-listp obj2)
          (p:picksetp obj2)
          (p:ename-listp obj2)))
      (setq res (inter-objlists obj1 obj2)))
    (t (setq res nil)))
  res)
```
</details>

---

### curve:join

**说明**: 合并多段线函数

**用法**:
```lisp
(curve:join entlst fuzz)
```

**参数**: 1 entlst  : 单个图元;2 fuzz  : 容差;

**返回值**: return:合并后的多段线图元名

**示例**:
```lisp
example:(curve:join '(ent1 ent2 ent3 ..) 0.000001)   (curve:join (ssget) 0.000001)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:join (entlst fuzz)
  "合并多段线函数"
  "return:合并后的多段线图元名"
  "example:(curve:join '(ent1 ent2 ent3 ..)
    0.000001)\n   (curve:join (ssget)
    0.000001)\n"
  (setq oldpeditaccept (getvar "PEDITACCEPT"))
  (setvar "PEDITACCEPT"
    1)
  (if (= fuzz nil)
    (setq fuzz 1.0e-006))
  (if (not (p:picksetp entlst))
    (cond ((p:ename-listp entlst)
        (setq entlst (entlist->pickset entlst)))
      ((p:vla-listp entlst)
        (setq entlst (entlist->pickset (mapcar (quote vla->e)
              entlst))))))
  (command "_.pedit"
    "_M"
    entlst ""
    "_J"
    "_J"
    "_B"
    fuzz "")
  (setvar "cmdecho"
    0)
  (setvar "PEDITACCEPT"
    oldpeditaccept)
  (entlast))
```
</details>

---

### curve:length

**说明**: 参数curve:曲线，直线、圆弧、圆、多段线、优化多段线、样条曲线,多线（控制点）等图元

**用法**:
```lisp
(curve:length curve)
```

**参数**: 1 curve  : 曲线;

**返回值**: 曲线的长度

**示例**:
```lisp
(curve:Length (car (entsel)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:length (curve)
  "参数curve:曲线，直线、圆弧、圆、多段线、优化多段线、样条曲线,多线（控制点）等图元"
  "曲线的长度"
  "(curve:Length (car (entsel)))"
  (if (= (quote ename)
      (type curve))
    (setq curve (e2o curve)))
  (cond ((string-equal "AcDbMline"
        (vla-get-objectname curve))
      (setq pts (curve:get-points curve))
      (setq len 0)
      (while (>= (length pts)
          2)
        (setq len (+ len (distance (car pts)
              (cadr pts))))
        (setq pts (cdr pts)))
      len)
    (t (vlax-curve-getdistatparam curve (vlax-curve-getendparam curve)))))
```
</details>

---

### curve:lwpl-is-circle-p

**说明**: 检测多段线是否为圆

**用法**:
```lisp
(curve:lwpl-is-circle-p ent)
```

**参数**: 1 ent  : 单个图元;

**返回值**: T or nil

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:lwpl-is-circle-p (ent / vertexs bulges flag o)
  "检测多段线是否为圆"
  "T or nil"
  (setq vertexs (curve:pline-3dpoints ent))
  (setq bulges (curve:pline-convexity ent))
  (if (> (length vertexs)
      (length bulges))
    (setq vertexs (reverse (cdr (reverse vertexs)))))
  (setq flag t)
  (setq o (curve:bulge2o (car vertexs)
      (cadr vertexs)
      (car bulges)))
  (repeat (length vertexs)
    (if (> (distance o (curve:bulge2o (car vertexs)
            (cadr vertexs)
            (car bulges)))
        0.001)
      (setq flag nil))
    (setq vertexs (append (cdr vertexs)
        (list (car vertexs))))
    (setq bulges (append (cdr bulges)
        (list (car bulges)))))
  flag)
```
</details>

---

### curve:lwpl-turn-clockwise

**说明**: 反转多段线，调整顺时针或逆时针方向。

**用法**:
```lisp
(curve:lwpl-turn-clockwise ent)
```

**参数**: 1 ent  : 单个图元;

**返回值**: 新多段线图元

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:lwpl-turn-clockwise (ent / pts convexity)
  "反转多段线，调整顺时针或逆时针方向。"
  "新多段线图元"
  (setq pts (curve:pline-3dpoints ent))
  (setq convexity (curve:pline-convexity ent))
  (if (> (length pts)
      (entity:getdxf ent 90))
    (setq pts (reverse (cdr (reverse pts)))))
  (if (> (length convexity)
      (entity:getdxf ent 90))
    (setq convexity (reverse (cdr (reverse convexity)))))
  (setq pts (reverse pts))
  (setq convexity (mapcar (quote -)
      (reverse convexity)))
  (setq convexity (append (cdr convexity)
      (list (car convexity))))
  (entity:make-lwpline-bold pts convexity nil 1 0))
```
</details>

---

### curve:midpoint

**说明**: 求曲线中点

**用法**:
```lisp
(curve:midpoint curve)
```

**参数**: 1 curve  : 曲线;

**返回值**: 中点坐标

**示例**:
```lisp
(curve:midpoint (car (entsel)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:midpoint (curve)
  "求曲线中点"
  "中点坐标"
  "(curve:midpoint (car (entsel)))"
  (if (= (quote vla-object)
      (type curve))
    (setq curve (o2e curve)))
  (if (= (quote ename)
      (type curve))
    (cond ((= "MLINE"
          (entity:getdxf curve 0))
        (setq midlen (* 0.5 (curve:length curve)))
        (setq pts (curve:get-points curve))
        (while (< (distance (car pts)
              (cadr pts))
            midlen)
          (setq midlen (- midlen (distance (car pts)
                (cadr pts))))
          (setq pts (cdr pts)))
        (setq pts (cdr pts))
        (polar (car pts)
          (angle (car pts)
            (cadr pts))
          (- midlen (distance (car pts)
              (cadr pts)))))
      (t (setq o-curve (e2o curve))
        (vlax-curve-getpointatdist o-curve (/ (curve:length curve)
            2))))))
```
</details>

---

### curve:o2bulge

**说明**: 求两点 pt1 pt2 和 圆心 O 和表示的弧的凸度。目前暂时没有考虑方向，及正负。

**用法**:
```lisp
(curve:o2bulge pt1 pt2 o)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;2 pt2  : 单个2D/3D坐标点;3 o  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:o2bulge (pt1 pt2 o / l)
  "求两点 pt1 pt2 和 圆心 O 和表示的弧的凸度。目前暂时没有考虑方向，及正负。"
  (/ (if (> (geometry:turn-right-p pt1 o pt2)
        0)
      (+ (distance o pt1)
        (distance o (point:mid pt1 pt2)))
      (- (distance o pt1)
        (distance o (point:mid pt1 pt2))))
    (* 0.5 (distance pt1 pt2))))
```
</details>

---

### curve:o2bulge-clockwise

**说明**: 求两点 pt1 pt2 和 圆心 O 表示的顺时针弧的凸度。

**用法**:
```lisp
(curve:o2bulge-clockwise pt1 pt2 o)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;2 pt2  : 单个2D/3D坐标点;3 o  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:o2bulge-clockwise (pt1 pt2 o / l)
  "求两点 pt1 pt2 和 圆心 O 表示的顺时针弧的凸度。"
  (* -1 (/ (if (> (geometry:turn-right-p pt1 o pt2)
          0)
        (- (distance o pt1)
          (distance o (point:mid pt1 pt2)))
        (+ (distance o pt1)
          (distance o (point:mid pt1 pt2))))
      (* 0.5 (distance pt1 pt2)))))
```
</details>

---

### curve:optimize-lwpl

**说明**: 优化多段线顶点。当连续多点共线或共圆时，减少顶点。优化顺时针的多段线有问题待修复。优化会丢失宽度信息！！

**用法**:
```lisp
(curve:optimize-lwpl ent)
```

**参数**: 1 ent  : 单个图元;

**返回值**: 优化后的新图元

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:optimize-lwpl (ent / segs res n1 n2 n3 fuzz)
  "优化多段线顶点。当连续多点共线或共圆时，减少顶点。优化顺时针的多段线有问题待修复。优化会丢失宽度信息！！"
  "优化后的新图元"
  (if (numberp tmp-fuzz)
    (setq fuzz tmp-fuzz)
    (setq fuzz 1.0e-08))
  (setq segs (mapcar (quote (lambda (x y)
          (cons x y)))
      (curve:pline-3dpoints ent)
      (curve:pline-convexity ent)))
  (setq n1 (car segs))
  (setq res (cons n1 nil))
  (setq segs (cdr segs))
  (setq n2 (car segs))
  (setq res (cons n2 res))
  (setq segs (cdr segs))
  (while (setq n3 (car segs))
    (cond ((and (= 0 (cdr n1))
          (= 0 (cdr n2))
          (equal (angle (car n1)
              (car n2))
            (angle (car n2)
              (car n3))
            fuzz)
          (setq res (cons n3 (cdr res)))))
      ((and (/= 0 (cdr n1))
          (/= 0 (cdr n2))
          (equal 0 (distance (curve:bulge2o (car n1)
                (car n2)
                (cdr n1))
              (curve:bulge2o (car n2)
                (car n3)
                (cdr n2)))
            fuzz))
        (setq res (cons (cons (car n1)
              (curve:o2bulge (car n1)
                (car n3)
                (curve:bulge2o (car n1)
                  (car n2)
                  (cdr n1))))
            (cddr res)))
        (setq res (cons n3 res)))
      (t (setq res (cons n3 res))))
    (setq n1 (cadr res))
    (setq n2 (car res))
    (setq segs (cdr segs)))
  (if (= 1 (entity:getdxf ent 70))
    (progn (setq n3 (last res))
      (cond ((and (= 0 (cdr n1))
            (= 0 (cdr n2))
            (equal (angle (car n1)
                (car n2))
              (angle (car n2)
                (car n3))
              fuzz)
            (setq res (cdr res))))
        ((and (/= 0 (cdr n1))
            (/= 0 (cdr n2))
            (equal 0 (distance (curve:bulge2o (car n1)
                  (car n2)
                  (cdr n1))
                (curve:bulge2o (car n2)
                  (car n3)
                  (cdr n2)))
              fuzz))
          (setq res (cons (cons (car n1)
                (curve:o2bulge (car n1)
                  (car n3)
                  (curve:bulge2o (car n1)
                    (car n2)
                    (cdr n1))))
              (cddr res)))))
      (setq n1 (car res))
      (setq n2 (last res))
      (setq n3 (cadr (reverse res)))
      (cond ((and (= 0 (cdr n1))
            (= 0 (cdr n2))
            (equal (angle (car n1)
                (car n2))
              (angle (car n2)
                (car n3))
              fuzz)
            (setq res (reverse (cdr (reverse res))))))
        ((and (/= 0 (cdr n1))
            (/= 0 (cdr n2))
            (equal 0 (distance (curve:bulge2o (car n1)
                  (car n2)
                  (cdr n1))
                (curve:bulge2o (car n2)
                  (car n3)
                  (cdr n2)))
              fuzz))
          (setq res (cons (cons (car n1)
                (curve:o2bulge (car n1)
                  (car n3)
                  (curve:bulge2o (car n1)
                    (car n2)
                    (cdr n1))))
              (cddr res)))))))
  (setq res (reverse res))
  (entity:make-lwpline-bold (mapcar (quote car)
      res)
    (mapcar (quote cdr)
      res)
    0 (entity:getdxf ent 70)
    0))
```
</details>

---

### curve:param-firstangle

**说明**: 曲线参数param处的切线方向的角度

**用法**:
```lisp
(curve:param-firstangle obj param)
```

**参数**: 1 obj  : activeX 对象;2 param  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:param-firstangle (obj param / pt)
  "曲线参数param处的切线方向的角度"
  (setq pt (vlax-curve-getpointatparam obj param))
  (angle (quote (0 0 0))
    (vlax-curve-getfirstderiv obj param)))
```
</details>

---

### curve:param-secondangle

**说明**: 曲线参数param处的法线方向的角度

**用法**:
```lisp
(curve:param-secondangle obj param)
```

**参数**: 1 obj  : activeX 对象;2 param  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:param-secondangle (obj param / pt)
  "曲线参数param处的法线方向的角度"
  (setq pt (vlax-curve-getpointatparam obj param))
  (angle (quote (0 0 0))
    (vlax-curve-getsecondderiv obj param)))
```
</details>

---

### curve:pickclosepointto

**说明**: 多段线上距离点击点最近的一个顶点

**用法**:
```lisp
(curve:pickclosepointto obj p)
```

**参数**: 1 obj  : activeX 对象;2 p  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:pickclosepointto (obj p / p1 p2 pp)
  "多段线上距离点击点最近的一个顶点"
  (setq pp (curve:subsegment-picked-points obj p))
  (setq p1 (car pp))
  (setq p2 (cadr pp))
  (if (< (distance p p1)
      (distance p p2))
    p1 p2))
```
</details>

---

### curve:pline-2dpoints

**说明**: 多段线端点列表，返回二维点坐标,LWPOLYLINE组码本来就是二维点。

**用法**:
```lisp
(curve:pline-2dpoints ent)
```

**参数**: 1 ent  : 单个图元;

**返回值**: 二维点坐标列表

**示例**:
```lisp
(curve:Pline-2dpoints (car (entsel)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:pline-2dpoints (ent)
  "多段线端点列表，返回二维点坐标,LWPOLYLINE组码本来就是二维点。"
  "二维点坐标列表"
  "(curve:Pline-2dpoints (car (entsel)))"
  (mapcar (quote cdr)
    (vl-remove-if-not (quote (lambda (x)
          (or (= (car x)
              10)
            (= (car x)
              11))))
      (entget ent))))
```
</details>

---

### curve:pline-3dpoints

**说明**: 多段线端点列表，返回三维点坐标。注意：当多段线为闭合时，最终点为起始点，比图元多一个点。

**用法**:
```lisp
(curve:pline-3dpoints ent)
```

**参数**: 1 ent  : 单个图元;

**返回值**: 三维点坐标列表

**示例**:
```lisp
(curve:pline-3dpoints (car (entsel)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:pline-3dpoints (ent / i lst v)
  "多段线端点列表，返回三维点坐标。注意：当多段线为闭合时，最终点为起始点，比图元多一个点。"
  "三维点坐标列表"
  "(curve:pline-3dpoints (car (entsel)))"
  (cond ((and (= (quote ename)
          (type ent))
        (= "LINE"
          (entity:getdxf ent 0)))
      (list (entity:getdxf ent 10)
        (entity:getdxf ent 11)))
    (t (setq i -1)
      (while (setq v (vlax-curve-getpointatparam ent (setq i (1+ i))))
        (setq lst (cons v lst)))
      (reverse lst))))
```
</details>

---

### curve:pline-convexity

**说明**: 多段线凸度列表。

**用法**:
```lisp
(curve:pline-convexity ent)
```

**参数**: 1 ent  : 单个图元;

**返回值**: 数值列表

**示例**:
```lisp
(curve:pline-convexity (car (entsel)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:pline-convexity (ent / i lst v)
  "多段线凸度列表。"
  "数值列表"
  "(curve:pline-convexity (car (entsel)))"
  (cond ((and (= (quote ename)
          (type ent))
        (= "LINE"
          (entity:getdxf ent 0)))
      (list 0))
    (t (mapcar (quote cdr)
        (vl-remove-if-not (quote (lambda (x)
              (= 42 (car x))))
          (entget ent))))))
```
</details>

---

### curve:point-firstangle

**说明**: 曲线一点的切线方向的角度

**用法**:
```lisp
(curve:point-firstangle obj pt)
```

**参数**: 1 obj  : activeX 对象;2 pt  : 单个2D/3D坐标点;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:point-firstangle (obj pt / param)
  "曲线一点的切线方向的角度"
  (setq param (vlax-curve-getparamatpoint obj pt))
  (angle (quote (0 0 0))
    (vlax-curve-getfirstderiv obj param)))
```
</details>

---

### curve:point-secondangle

**说明**: 曲线一点的法线方向的角度

**用法**:
```lisp
(curve:point-secondangle obj pt)
```

**参数**: 1 obj  : activeX 对象;2 pt  : 单个2D/3D坐标点;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:point-secondangle (obj pt / param)
  "曲线一点的法线方向的角度"
  (setq param (vlax-curve-getparamatpoint obj pt))
  (angle (quote (0 0 0))
    (vlax-curve-getsecondderiv obj param)))
```
</details>

---

### curve:pt-in-arc-p

**说明**: 判断 点 pt 是否在 pt1 pt2 及 凸度 表示的圆弧上。

**用法**:
```lisp
(curve:pt-in-arc-p pt pt1 pt2 convexity)
```

**参数**: 1 pt  : 单个2D/3D坐标点;2 pt1  : 单个2D/3D坐标点;3 pt2  : 单个2D/3D坐标点;4 convexity  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:pt-in-arc-p (pt pt1 pt2 convexity / o ang1 ang2 ang)
  "判断 点 pt 是否在 pt1 pt2 及 凸度 表示的圆弧上。"
  (if (equal 0 convexity 0.001)
    (progn (equal (angle pt1 pt)
        (angle pt pt2)
        0.001))
    (progn (setq o (curve:bulge2o pt1 pt2 convexity))
      (setq ang1 (angle o pt1)
        ang2 (angle o pt2)
        ang (angle o pt))
      (and (equal (distance pt o)
          (distance pt1 o)
          0.001)
        (if (> convexity 0)
          (if (> ang1 ang2)
            (or (> ang ang1)
              (> ang2 ang))
            (> ang2 ang ang1))
          (if (> ang1 ang2)
            (> ang1 ang ang2)
            (or (> ang1 ang)
              (> ang ang2))))))))
```
</details>

---

### curve:ptoncurve

**说明**: 判断点是否在曲线上

**用法**:
```lisp
(curve:ptoncurve pt curve)
```

**参数**: 1 pt  : 单个2D/3D坐标点;2 curve  : 曲线;

**返回值**: T or nil

**示例**:
```lisp
(curve:PtOnCurve (getpoint) (car (entsel)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:ptoncurve (pt curve)
  "判断点是否在曲线上"
  "T or nil"
  "(curve:PtOnCurve (getpoint)
    (car (entsel)))"
  (equal pt (vlax-curve-getclosestpointto curve pt)
    1.0e-005))
```
</details>

---

### curve:put-points

**说明**: 更改曲线控制点及端点列表。pts为新的点位置,nil为不替换。

**用法**:
```lisp
(curve:put-points ent pts)
```

**参数**: 1 ent  : 单个图元;2 pts  : 多个坐标点列表;

**返回值**: ent

**示例**:
```lisp
(curve:put-points (car (entsel)) '((0 0 0)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:put-points (ent pts / dxfent)
  "更改曲线控制点及端点列表。pts为新的点位置,nil为不替换。"
  "ent"
  "(curve:put-points (car (entsel))
    '((0 0 0)))"
  (if (p:vlap ent)
    (setq ent (o2e ent)))
  (if (= (quote ename)
      (type ent))
    (cond ((wcmatch (entity:getdxf ent 0)
          "*POLYLINE,LINE,MLINE,CIRCLE,SPLINE,REGION,ELLIPSE,ARC")
        (setq dxfent (entget ent))
        (mapcar (quote (lambda (dxf1011 pt)
              (if pt (setq dxfent (subst (cons (car dxf1011)
                      pt)
                    dxf1011 dxfent)))))
          (vl-remove-if-not (quote (lambda (x)
                (or (= 10 (car x))
                  (= 11 (car x)))))
            (entget ent))
          pts)
        (entmod dxfent)
        (entupd ent)))))
```
</details>

---

### curve:putclosed

**说明**: 使多段线封闭

**用法**:
```lisp
(curve:putclosed obj)
```

**参数**: 1 obj  : activeX 对象;

**返回值**: 无

**示例**:
```lisp
(curve:putClosed (car (entsel)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:putclosed (obj)
  "使多段线封闭"
  "无"
  "(curve:putClosed (car (entsel)))"
  (or (p:vlap obj)
    (setq obj (vlax-ename->vla-object obj)))
  (if (not (vlax-curve-isclosed obj))
    (vla-put-closed obj :vlax-true)))
```
</details>

---

### curve:readme

**说明**: 曲线操作相关函数。

**用法**:
```lisp
(curve:readme )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:readme nil "曲线操作相关函数。"
  (princ "曲线操作相关函数。使用 (require 'curve:*)
    加载这些函数")
  (princ))
```
</details>

---

### curve:rectangle-center

**说明**: 矩形中点坐标

**用法**:
```lisp
(curve:rectangle-center en)
```

**参数**: 1 en  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:rectangle-center (en / pl)
  "矩形中点坐标"
  (setq pl (curve:pline-2dpoints en))
  (mapcar (quote (lambda (x y)
        (/ (+ x y)
          2.0)))
    (car pl)
    (caddr pl)))
```
</details>

---

### curve:rectanglep

**说明**: 测试一个多段线是否为矩形,判断矩形

**用法**:
```lisp
(curve:rectanglep ent)
```

**参数**: 1 ent  : 单个图元;

**返回值**: T or nil

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:rectanglep (ent)
  "测试一个多段线是否为矩形,判断矩形"
  "T or nil"
  (and (= (quote ename)
      (type ent))
    (wcmatch (entity:getdxf ent 0)
      "*POLYLINE")
    (or (and (= (entity:getdxf ent 90)
          4)
        (= (entity:getdxf ent 70)
          1))
      (and (= (entity:getdxf ent 90)
          5)
        (= (entity:getdxf ent 70)
          0)))
    (apply (quote =)
      (entity:getdxf ent 42))
    (progn (setq pts (curve:get-points ent))
      (setq ang (abs (- (angle (nth 0 pts)
              (nth 1 pts))
            (angle (nth 1 pts)
              (nth 2 pts)))))
      (if (> ang pi)
        (setq ang (- ang pi)))
      (and (equal ang (* pi 0.5)
          1.0e-06)
        (equal (distance (nth 0 pts)
            (nth 1 pts))
          (distance (nth 2 pts)
            (nth 3 pts))
          1.0e-06)
        (equal (distance (nth 0 pts)
            (nth 2 pts))
          (distance (nth 1 pts)
            (nth 3 pts))
          1.0e-06)
        (if (nth 4 pts)
          (< (distance (nth 0 pts)
              (nth 4 pts))
            1.0e-06)
          t)))))
```
</details>

---

### curve:similar-p

**说明**: 判定两曲线是否相似,curve1,curve2 曲线图元,默认相似度:95%

**用法**:
```lisp
(curve:similar-p curve1 curve2)
```

**参数**: 1 curve1  : 曲线;2 curve2  : 曲线;

**返回值**: bool

**示例**:
```lisp
(curve:similar-p (car(entsel))(car(entsel)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:similar-p (curve1 curve2 / similarity o1 o2 nolength)
  "判定两曲线是否相似,curve1,curve2 曲线图元,默认相似度:95%"
  "bool"
  "(curve:similar-p (car(entsel))(car(entsel)))"
  (setq nolength (quote ("CIRCLE"
        "ARC"
        "SPLINE"
        "ELLIPSE"
        "REGION")))
  (if (@::get-config (quote curve:similarity))
    (setq similarity (- 1 (@::get-config (quote curve:similarity))))
    (setq similarity 0.05))
  (setq o1 (e2o curve1)
    o2 (e2o curve2))
  (and (if (or (member (entity:getdxf curve1 0)
          nolength)
        (member (entity:getdxf curve2 0)
          nolength))
      t (cond ((equal (vla-get-length o1)
            (vla-get-length o2)
            1.0e-06)
          t)
        ((/= (vla-get-length o2)
            0)
          (equal (/ (float (vla-get-length o1))
              (float (vla-get-length o2)))
            1 similarity))))
    (progn (setq area1 (if (= "LINE"
            (entity:getdxf curve1 0))
          0 (vla-get-area o1)))
      (setq area2 (if (= "LINE"
            (entity:getdxf curve2 0))
          0 (vla-get-area o2)))
      (cond ((equal area1 area2 1.0e-06)
          t)
        ((/= area2 0)
          (equal (/ area1 area2)
            1 similarity))
        (t nil)))))
```
</details>

---

### curve:subsegment-length

**说明**: 多段线子段长度，当曲线闭合且 pt2 与起点相同时,pt2按终点考虑

**用法**:
```lisp
(curve:subsegment-length curve pt1 pt2)
```

**参数**: 1 curve  : 曲线;2 pt1  : 单个2D/3D坐标点;3 pt2  : 单个2D/3D坐标点;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:subsegment-length (curve pt1 pt2)
  "多段线子段长度，当曲线闭合且 pt2 与起点相同时,pt2按终点考虑"
  (if (p:enamep curve)
    (setq curve (e2o curve)))
  (cond ((equal 0 (distance (vlax-curve-getendpoint curve)
          pt2)
        1.0e-10)
      (- (vla-get-length curve)
        (vlax-curve-getdistatpoint curve pt1)))
    (t (cond ((and (listp pt1)
            (listp pt2))
          (abs (- (vlax-curve-getdistatpoint curve pt1)
              (vlax-curve-getdistatpoint curve pt2))))
        (t (abs (- (vlax-curve-getdistatparam curve pt1)
              (vlax-curve-getdistatparam curve pt2))))))))
```
</details>

---

### curve:subsegment-parameter

**说明**: 多段线子段参数

**用法**:
```lisp
(curve:subsegment-parameter curve pt)
```

**参数**: 1 curve  : 曲线;2 pt  : 单个2D/3D坐标点;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:subsegment-parameter (curve pt / arclength cenangle center points pt1 pt1param pt2 pt3 radius tlength xangle xbulge)
  "多段线子段参数"
  (setq points (curve:subsegment-picked-points curve pt)
    pt1 (car points)
    pt2 (cadr points)
    pt1param (vlax-curve-getparamatpoint curve pt1)
    arclength (curve:subsegment-length curve pt1 pt2))
  (setq xbulge (vla-getbulge (vlax-ename->vla-object curve)
      pt1param))
  (if (= 0 xbulge)
    (progn (list pt1 pt2 arclength (angle pt1 pt2)))
    (progn (setq xangle (* 4 (atan xbulge))
        cenangle ((if (< xbulge 0)
            - +)
          (- (angle pt1 pt2)
            (/ xangle 2.0))
          (/ pi 2))
        radius (abs (/ (/ (distance pt1 pt2)
              2.0)
            (sin (/ xangle 2.0))))
        center (polar pt1 cenangle radius)
        pt3 (inters pt1 (polar pt1 (+ (angle pt1 center)
              (/ pi 2))
            radius)
          pt2 (polar pt2 (+ (angle pt2 center)
              (/ pi 2))
            radius)
          nil)
        tlength (if (null pt3)
          0 (distance pt2 pt3)))
      (list center pt1 pt2 pt3 radius (abs xangle)
        arclength tlength))))
```
</details>

---

### curve:subsegment-picked-param

**说明**: 多段线所点击子段参数

**用法**:
```lisp
(curve:subsegment-picked-param obj p)
```

**参数**: 1 obj  : activeX 对象;2 p  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:subsegment-picked-param (obj p / pp)
  "多段线所点击子段参数"
  (setq pp (vlax-curve-getclosestpointto obj (trans p 1 0)))
  (fix (vlax-curve-getparamatpoint obj pp)))
```
</details>

---

### curve:subsegment-picked-points

**说明**: 多段线所点击子段的两端点列表

**用法**:
```lisp
(curve:subsegment-picked-points obj p)
```

**参数**: 1 obj  : activeX 对象;2 p  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:subsegment-picked-points (obj p)
  "多段线所点击子段的两端点列表"
  (curve:subsegment-points obj (fix (vlax-curve-getparamatpoint obj (vlax-curve-getclosestpointto obj (trans p 1 0))))))
```
</details>

---

### curve:subsegment-picked-type

**说明**: 多段线子段图元类型

**用法**:
```lisp
(curve:subsegment-picked-type curve p)
```

**参数**: 1 curve  : 曲线;2 p  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:subsegment-picked-type (curve p / pp)
  "多段线子段图元类型"
  (if (listp p)
    (progn (setq pp (vlax-curve-getclosestpointto curve (trans p 1 0)))
      (setq pp (vlax-curve-getsecondderiv curve (fix (vlax-curve-getparamatpoint curve pp)))))
    (setq pp (vlax-curve-getsecondderiv curve p)))
  (if (equal pp (quote (0.0 0.0 0.0)))
    "line"
    "arc"))
```
</details>

---

### curve:subsegment-points

**说明**: 多段线第n子段的端点坐标

**用法**:
```lisp
(curve:subsegment-points curve n)
```

**参数**: 1 curve  : 曲线;2 n  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:subsegment-points (curve n)
  "多段线第n子段的端点坐标"
  (list (vlax-curve-getpointatparam curve (fix n))
    (vlax-curve-getpointatparam curve (1+ (fix n)))))
```
</details>

---

### curve:subsegments

**说明**: 多段线子段数

**用法**:
```lisp
(curve:subsegments obj)
```

**参数**: 1 obj  : activeX 对象;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun curve:subsegments (obj)
  "多段线子段数"
  (fix (vlax-curve-getendparam obj)))
```
</details>

---

## datetime

**模块说明**: 日期时间处理、格式化

### datetime:current-time

**说明**: 格式化日期时间，yyyy 年 mo 月 dd 日 hh 时 mm 分 ss 秒

**用法**:
```lisp
(datetime:current-time str-fmt)
```

**参数**: 1 str-fmt  : 字符串;

**返回值**: 日期时间字符串

**示例**:
```lisp
(datetime:current-time "yyyy-mo-dd hh:mm:ss")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun datetime:current-time (str-fmt)
  "格式化日期时间，yyyy 年 mo 月 dd 日 hh 时 mm 分 ss 秒"
  "日期时间字符串"
  "(datetime:current-time \"yyyy-mo-dd hh:mm:ss\")"
  (menucmd (strcat "M=$(edtime,$(getvar,date),"
        str-fmt ")")))
```
</details>

---

### datetime:get-current-day

**说明**: 返回日期

**用法**:
```lisp
(datetime:get-current-day )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun datetime:get-current-day nil "返回日期"
  (substr (itoa (fix (getvar "CDATE")))
    7 2))
```
</details>

---

### datetime:get-current-month

**说明**: 返回月份

**用法**:
```lisp
(datetime:get-current-month )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun datetime:get-current-month nil "返回月份"
  (substr (itoa (fix (getvar "CDATE")))
    5 2))
```
</details>

---

### datetime:get-current-year

**说明**: 返回年份

**用法**:
```lisp
(datetime:get-current-year )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun datetime:get-current-year nil "返回年份"
  (substr (itoa (fix (getvar "CDATE")))
    1 4))
```
</details>

---

### datetime:get-universal-time

**说明**: 返回世界时，格林维治时间.自1900年1月1日到现在的秒数

**用法**:
```lisp
(datetime:get-universal-time )
```

**参数**: None

**返回值**: Int

**示例**:
```lisp
(datetime:get-universal-time)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun datetime:get-universal-time nil "返回世界时，格林维治时间.自1900年1月1日到现在的秒数"
  "Int"
  "(datetime:get-universal-time)"
  (if (@:internetp)
    (datetime:mktime1900 (datetime:mktime (datetime:rfc1123-to-lisp (vlax-invoke @:*request* (quote getresponseheader)
            "Date"))))
    0))
```
</details>

---

### datetime:leap-yearp

**说明**: 判断某个是否为闰年。

**用法**:
```lisp
(datetime:leap-yearp year)
```

**参数**: 1 year  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun datetime:leap-yearp (year)
  "判断某个是否为闰年。"
  (if (= 0 (mod year 100))
    (if (= 0 (mod year 400))
      t nil)
    (if (= 0 (mod year 4))
      t nil)))
```
</details>

---

### datetime:mktime

**说明**: 计算某一时间(列表)到1970年01月01日经过的秒数,适合转换vl-file-systime的结果

**用法**:
```lisp
(datetime:mktime lst)
```

**参数**: 1 lst  : 列表;

**返回值**: Timestamp

**示例**:
```lisp
(datetime:mktime (vl-file-systime (findfile "acad.pgp")))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun datetime:mktime (lst /)
  "计算某一时间(列表)到1970年01月01日经过的秒数,适合转换vl-file-systime的结果"
  "Timestamp"
  "(datetime:mktime (vl-file-systime (findfile \"acad.pgp\")))"
  (setq days-of-month (if (datetime:leap-yearp (nth 0 lst))
      (quote (0 31 60 91 121 152 182 213 244 274 305 335 366))
      (quote (0 31 59 90 120 151 181 212 243 273 304 334 365))))
  (+ (* 60.0 (+ (* 60.0 (+ (* 24.0 (+ (* (- (nth 0 lst)
                    1970.0)
                  365.0)
                (fix (/ (- (nth 0 lst)
                      1970)
                    4))
                (nth (1- (nth 1 lst))
                  days-of-month)
                (nth 3 lst)))
            (nth 4 lst)
            (- 8)))
        (nth 5 lst)))
    (nth 6 lst)))
```
</details>

---

### datetime:mktime1900

**说明**: unix timestamp 转 到1900年01月01日经过的秒数.

**用法**:
```lisp
(datetime:mktime1900 timestamp)
```

**参数**: 1 timestamp  : 未明确定义;

**返回值**: real

**示例**:
```lisp
(datetime:mktime1900 (datetime:mktime (vl-file-systime (findfile "acad.pgp"))))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun datetime:mktime1900 (timestamp)
  "unix timestamp 转 到1900年01月01日经过的秒数."
  "real"
  "(datetime:mktime1900 (datetime:mktime (vl-file-systime (findfile \"acad.pgp\"))))"
  (+ (* 22089.0 100000.0)
    88800.0 timestamp))
```
</details>

---

### datetime:rfc1123-to-lisp

**说明**: 将RFC1123格式转化为 autolisp 表格式

**用法**:
```lisp
(datetime:rfc1123-to-lisp str)
```

**参数**: 1 str  : 字符串;

**返回值**: list

**示例**:
```lisp
(datetime:rfc1123-to-lisp "Mon, 12 Sep 2022 03:58:42 GMT")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun datetime:rfc1123-to-lisp (str)
  "将RFC1123格式转化为 autolisp 表格式"
  "list"
  "(datetime:rfc1123-to-lisp \"Mon, 12 Sep 2022 03:58:42 GMT\")"
  (setq mon (quote ("Jan"
        "Feb"
        "Mar"
        "Apr"
        "May"
        "Jun"
        "Jul"
        "Aug"
        "Sep"
        "Oct"
        "Nov"
        "Dec")))
  (setq week (mapcar (function (lambda (x)
          (substr x 1 3)))
      (quote ("Monday"
          "Tuesday"
          "Wednesday"
          "Thurday"
          "Friday"
          "Saturday"
          "Sunday"))))
  (setq res (string:parse-by-lst str (quote ("
          "
          ":"))))
  (list (atoi (nth 3 res))
    (1+ (vl-position (nth 2 res)
        mon))
    (1+ (vl-position (substr (nth 0 res)
          1 3)
        week))
    (atoi (nth 1 res))
    (atoi (nth 4 res))
    (atoi (nth 5 res))
    (atoi (nth 6 res))
    0))
```
</details>

---

## dbx

**模块说明**: 专用功能模块

### dbx:interface

**说明**: 返回DBX的实例化接口

**用法**:
```lisp
(dbx:interface )
```

**参数**: 没有一个

**返回值**: Obj

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dbx:interface nil "返回DBX的实例化接口"
  "Obj"
  (if (or (null *dbx*)
      (vlax-object-released-p *dbx*))
    (setq *dbx* (vla-getinterfaceobject *acad* (strcat "ObjectDBX.AxDbDocument."
          (itoa (fix (@:acadver))))))))
```
</details>

---

### dbx:open

**说明**: 以DBX方式打开dwg文件

**用法**:
```lisp
(dbx:open dwg-file)
```

**参数**: 1 dwg-file  : 未识别定义;

**返回值**: 打开成功返回DBX对象，没有发现文件返回nil

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dbx:open (dwg-file / dbx)
  "以DBX方式打开dwg文件"
  "打开成功返回DBX对象，没有发现文件返回nil"
  (dbx:interface)
  (if (findfile dwg-file)
    (progn (vla-open *dbx* (findfile dwg-file))
      *dbx*)))
```
</details>

---

### dbx:release

**说明**: 释放DBX接口

**用法**:
```lisp
(dbx:release )
```

**参数**: 没有一个

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dbx:release nil "释放DBX接口"
  (if (and *dbx* (= (quote vla-object)
        (type *dbx*))
      (null (vlax-object-released-p *dbx*)))
    (vlax-release-object *dbx*)))
```
</details>

---

## dcl

**模块说明**: 专用功能模块

### dcl:accept

**用法**:
```lisp
(dcl:accept )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:accept nil (setq dcl:dialog-result (mapcar (quote (lambda (x)
          (cons (car x)
            (eval (cdr x)))))
      dcl:accept-hook))
  (done_dialog 2))
```
</details>

---

### dcl:begin-cluster

**说明**: 开始 dcl 容器类控件。与 dcl:end-cluster 成对使用。cluster-type: row,column,boxed_row,boxed_column,boxed_radio_row,boxed_radio_column

**用法**:
```lisp
(dcl:begin-cluster cluster-type label)
```

**参数**: 1 cluster-type  : 未识别定义;2 label  : 标签;

**示例**:
```lisp
(dcl:begin-cluster "row"    "")(progn (dcl:mtext "mt"      8 100 )(dcl:paging t)    (dcl:end-cluster))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:begin-cluster (cluster-type label)
  "开始 dcl 容器类控件。与 dcl:end-cluster 成对使用。cluster-type: row,column,boxed_row,boxed_column,boxed_radio_row,boxed_radio_column"
  ""
  "(dcl:begin-cluster \"row\"\n    \"\")(progn (dcl:mtext \"mt\"\n      8 100 )(dcl:paging t)\n    (dcl:end-cluster))"
  (if (member cluster-type (quote ("row"
          "column"
          "radio_row"
          "radio_column"
          "boxed_row"
          "boxed_column"
          "boxed_radio_row"
          "boxed_radio_column")))
    (write-line (strcat ":"
        cluster-type "{"
        (if (/= ""
            label)
          (strcat "label=\""
            label "\";")
          ""))
      dcl-fp)
    (write-line ":row{"
      dcl-fp)))
```
</details>

---

### dcl:button

**说明**: dcl 按钮。

**用法**:
```lisp
(dcl:button key label style)
```

**参数**: 1 key  : 键，关键字;2 label  : 标签;3 style  : 未识别定义;

**示例**:
```lisp
(dcl:button "btn1" "Button1" "")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:button (key label style)
  "dcl 按钮。"
  ""
  "(dcl:button \"btn1\"
    \"Button1\"
    \"\")"
  (set (read (strcat "cb-"
        key))
    (eval (read (strcat "(lambda()(alert (strcat \"需要定义回调函数 (cb-"
                  key ")\")))"))))
  (write-line (strcat ":button{key=\""
      key "\";"
      "label=\""
      label "\";"
      "action=\"(cb-"
        key ")\";"
      style "}")
    dcl-fp))
```
</details>

---

### dcl:cell

**说明**: 创建 DCL 可编辑表格.参数: 1. key: 标识      2. rows:显示行数，      3. columns 显示列数(不大于26),       4. show-line-number? : 是否显示行号，       5. show-column-number?  : 是否显示列号A~Z，      6. show-scrollbar? : 是否显示竖向滚动条.       7. fixed-title? ,在第一行固定显示标题。tile-key: scrollbar header1 header2 headern, A1 B1 ... Z1 , A2 B2 ... Z2 , A99 B99 ... Z99 ......Model部: tbl-widths: 列宽表;cell-data 列表数据;Control部: (cb-scrollbar)

**用法**:
```lisp
(dcl:cell key rows columns show-line-number? show-column-number? show-scrollbar? header-title?)
```

**参数**: 1 key  : 键，关键字;2 rows  : 未识别定义;3 columns  : 未识别定义;4 show-line-number?  : 未识别定义;5 show-column-number?  : 未识别定义;6 show-scrollbar?  : 未识别定义;7 header-title?  : 未识别定义;

**返回值**: DCL格式字符串

**示例**:
```lisp
(dcl:cell "cell1" 10 8 t t t t)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:cell (key rows columns show-line-number? show-column-number? show-scrollbar? header-title? / tbl-widths *error*)
  "创建 DCL 可编辑表格.\n参数: 1. key: 标识\n      2. rows:显示行数，\n      3. columns 显示列数(不大于26), \n      4. show-line-number? : 是否显示行号， \n      5. show-column-number?  : 是否显示列号A~Z，\n      6. show-scrollbar? : 是否显示竖向滚动条. \n      7. fixed-title? ,在第一行固定显示标题。\n\ntile-key: scrollbar header1 header2 headern, A1 B1 ... Z1 , A2 B2 ... Z2 , A99 B99 ... Z99 ......\nModel部: tbl-widths: 列宽表;cell-data 列表数据;\nControl部: (cb-scrollbar)"
  "DCL格式字符串"
  "(dcl:cell \"cell1\"
    10 8 t t t t)"
  (defun *error* (msg)
    (if (/= (quote file)
        (type dcl-fp))
      (alert "请先运行 (dcl:dialog  ...)"))
    (@:*error* msg))
  (defun dcl:get-celldata (key /)
    "返回cell 表格的结果，会参照原始表进行数据转换。"
    ""
    ""
    (set (read (strcat key "-data"))
      (mapcar (quote (lambda (x type1)
            (mapcar (quote (lambda (y ty)
                  (if (p:stringp y)
                    (@:to-type y (type ty))
                    y)))
              x type1)))
        (dcl:save-celldata key)
        lst-cellraw)))
  (defun dcl:save-celldata (key / lst-dcl per-page% curr-page% tmp-data% tmp-data-pre tmp-data-last)
    "保存 cell 表的结果到临时表 *key*tmp-data 中。未进行类型转换，查看过的均为字符串类型。"
    ""
    ""
    (setq per-page% (eval (read (strcat key "per-page")))
      curr-page% (eval (read (strcat key "curr-page")))
      tmp-data% (eval (read (strcat key "tmp-data"))))
    (setq lst-dcl nil r% 0)
    (repeat (min per-page% (- (length (cdr tmp-data%))
          (* curr-page% per-page%)))
      (setq c% 64)
      (setq r% (1+ r%))
      (setq lst-r% (quote nil))
      (repeat (length (car tmp-data%))
        (setq lst-r% (cons (vl-string-left-trim (chr 32)
              (get_tile (strcat key (chr (setq c% (1+ c%)))
                  (itoa r%))))
            lst-r%)))
      (setq lst-dcl (cons (reverse lst-r%)
          lst-dcl)))
    (setq lst-dcl (reverse lst-dcl))
    (setq i% 0)
    (setq tmp-data-pre nil)
    (repeat (min (* curr-page% per-page%)
        (length (cdr tmp-data%)))
      (setq tmp-data-pre (cons (nth (setq i% (1+ i%))
            tmp-data%)
          tmp-data-pre)))
    (setq i% (* (1+ curr-page%)
        per-page%))
    (setq tmp-data-last (quote nil))
    (if (> (- (length (cdr tmp-data%))
          (* (1+ curr-page%)
            per-page%))
        0)
      (repeat (- (length (cdr tmp-data%))
          (* (1+ curr-page%)
            per-page%))
        (setq tmp-data-last (cons (nth (setq i% (1+ i%))
              tmp-data%)
            tmp-data-last))))
    (set (read (strcat key "tmp-data"))
      (setq tmp-data% (append (list (car tmp-data%))
          (reverse tmp-data-pre)
          lst-dcl (reverse tmp-data-last)))))
  (defun dcl:show-celldata (key / tmp-unit curr-page% tmp-data% per-page%)
    (setq per-page% (eval (read (strcat key "per-page")))
      curr-page% (eval (read (strcat key "curr-page")))
      tmp-data% (eval (read (strcat key "tmp-data"))))
    (setq i% 0)
    (repeat (length (car tmp-data%))
      (set_tile (strcat key "header"
          (itoa (setq i% (1+ i%))))
        (nth (1- i%)
          (car tmp-data%))))
    (setq c% 64)
    (repeat (length (car tmp-data%))
      (setq r% 0 c% (1+ c%))
      (repeat (min per-page% (- (length (cdr tmp-data%))
            (* per-page% curr-page%)))
        (setq tmp-unit (nth (- c% 65)
            (nth (1- (+ (setq r% (1+ r%))
                  (* per-page% curr-page%)))
              (cdr tmp-data%))))
        (set_tile (strcat key (chr c%)
            (itoa r%))
          (cond ((or (p:intp tmp-unit)
                (and (= (quote str)
                    (type tmp-unit))
                  (string:intp tmp-unit)))
              (string:number-format (vl-string-left-trim (chr 32)
                  (@:to-string tmp-unit))
                5 0 (chr 32)))
            ((or (numberp tmp-unit)
                (and (= (quote str)
                    (type tmp-unit))
                  (string:numberp tmp-unit)))
              (string:number-format (vl-string-left-trim (chr 32)
                  (@:to-string tmp-unit))
                5 3 (chr 32)))
            (t (@:to-string tmp-unit))))
        (mode_tile (strcat key (chr c%)
            (itoa r%))
          0)
        (set_tile (strcat key "rno"
            (itoa r%))
          (string:number-format (@:to-string (+ r% (* per-page% curr-page%)))
            7 0 (chr 32))))
      (while (< r% per-page%)
        (repeat (length (car tmp-data%))
          (set_tile (strcat key (chr c%)
              (itoa (setq r% (1+ r%))))
            "")
          (mode_tile (strcat key (chr c%)
              (itoa r%))
            1)
          (set_tile (strcat key "rno"
              (itoa r%))
            "")))))
  (setq dcl:accept-hook (cons (list key (quote dcl:get-celldata)
        key)
      dcl:accept-hook))
  (set (read (strcat key "per-page"))
    rows)
  (set (read (strcat key "total-page"))
    (fix (1+ (/ (1- (length (cdr (eval (read (strcat key "tmp-data"))))))
          (eval (read (strcat key "per-page")))))))
  (set (read (strcat "cb-"
        key "scrollbar"))
    (lambda (key)
      "滚动条事件的回调函数"
      (dcl:save-celldata key)
      (set (read (strcat key "curr-page"))
        (- (1- (eval (read (strcat key "total-page"))))
          (atoi (get_tile (strcat key "scrollbar")))))
      (print (eval (read (strcat key "curr-page"))))
      (dcl:show-celldata key)))
  (if (and ui:*table-widths* (apply (quote and)
        (mapcar (quote numberp)
          ui:*table-widths*))
      (> (apply (quote min)
          ui:*table-widths*)
        0)
      (= (length ui:*table-widths*)
        columns))
    (setq tbl-widths ui:*table-widths*)
    (progn (repeat rows (setq tbl-widths (cons 10 tbl-widths)))))
  (write-line ":row{:column{"
    dcl-fp)
  (if show-column-number? (progn (write-line (strcat ": row { ")
        dcl-fp)
      (if show-line-number? (write-line (strcat ":edit_box{value=\"No.\";width=2;fixed_width=true;horizontal_margin=none;vertical_margin=none;is_enabled=false;}")
          dcl-fp))
      (setq c% 64)
      (repeat columns (write-line (strcat ":edit_box{value=\""
            (chr (setq c% (1+ c%)))
            "\";width="
            (itoa (nth (- c% 65)
                tbl-widths))
            ";fixed_width=true;horizontal_margin=none;vertical_margin=none;is_enabled=false;}")
          dcl-fp))
      (write-line (strcat "}")
        dcl-fp)))
  (if header-title? (progn (write-line (strcat ": row { ")
        dcl-fp)
      (if show-line-number? (write-line (strcat ":edit_box{value=\"\";width=2;fixed_width=true;horizontal_margin=none;vertical_margin=none;is_enabled=false;}")
          dcl-fp))
      (setq i% 0)
      (repeat columns (write-line (strcat ":edit_box{key=\""
            key "header"
            (itoa (setq i% (1+ i%)))
            "\";value=\"\";width="
            (itoa (nth (1- i%)
                tbl-widths))
            ";fixed_width=true;horizontal_margin=none;vertical_margin=none;is_enabled=false;}")
          dcl-fp))
      (write-line (strcat "}")
        dcl-fp)))
  (setq r% 0)
  (repeat rows (write-line (strcat ":row{")
      dcl-fp)
    (setq r% (1+ r%)
      c% 64)
    (if show-line-number? (write-line (strcat ":edit_box{key=\""
          key "rno"
          (itoa r%)
          "\";value=\"\";width=2;fixed_width=true;horizontal_margin=none;vertical_margin=none;is_enabled=false;}")
        dcl-fp))
    (repeat columns (write-line (strcat ":edit_box{key=\""
          key (chr (setq c% (1+ c%)))
          (itoa r%)
          "\";value=\"\";width="
          (itoa (nth (- c% 65)
              tbl-widths))
          ";fixed_width=true;horizontal_margin=none;vertical_margin=none;}")
        dcl-fp))
    (write-line (strcat "}")
      dcl-fp))
  (write-line (strcat "}")
    dcl-fp)
  (if show-scrollbar? (write-line (strcat ":slider{action=\"(cb-"
          key "scrollbar \\\""
          key "\\\")\";key=\""
        key "scrollbar\";layout=vertical;value="
        (itoa (1- (eval (read (strcat key "total-page")))))
        ";max_value="
        (itoa (1- (eval (read (strcat key "total-page")))))
        ";min_value=0;small_increment=1;}")
      dcl-fp))
  (write-line "}"
    dcl-fp))
```
</details>

---

### dcl:dialog

**说明**: 创建名为 name 的对话框文件。外部变量: dcl-tmp 含路径的文件名. dcl-fp 文件指针。

**用法**:
```lisp
(dcl:dialog name)
```

**参数**: 1 name  : 未识别定义;

**示例**:
```lisp
(dcl:dialog "tips")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:dialog (name)
  "创建名为 name 的对话框文件。外部变量: dcl-tmp 含路径的文件名. dcl-fp 文件指针。 "
  ""
  "(dcl:dialog \"tips\")"
  (setq dcl:accept-hook nil)
  (setq dcl-tmp
    (cond
      ((eq $platform 'microsoft)
       (vl-filename-mktemp name @::*tmp-path* ".dcl"))
      ((eq $platform 'linux)
       (strcat "/tmp/tmp-"name".dcl"))))
  (setq dcl-fp (open dcl-tmp "w"))
  (write-line (strcat name ":dialog {"
      "label = \""
      name "\";key=\"title\";")
    dcl-fp))
```
</details>

---

### dcl:dialog-end-ok-cancel

**说明**: 完成创建 DCL文件 ， 并关闭文件指针。

**用法**:
```lisp
(dcl:dialog-end-ok-cancel )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:dialog-end-ok-cancel nil "完成创建 DCL文件 ， 并关闭文件指针。"
  ""
  (write-line ":spacer{} ok_cancel;}"
    dcl-fp)
  (close dcl-fp))
```
</details>

---

### dcl:end-cluster

**说明**: 开始 dcl 容器类控件。与 dcl:begin-cluster 成对使用。

**用法**:
```lisp
(dcl:end-cluster )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:end-cluster nil "开始 dcl 容器类控件。与 dcl:begin-cluster 成对使用。"
  ""
  ""
  (write-line "}"
    dcl-fp))
```
</details>

---

### dcl:end-dialog

**说明**: 完成创建 DCL文件 ， 并关闭文件指针。参数 str-Yes-No 字符串用 - 分隔成两部分，前面为accept,后面为 Cancel.如 是-否，愿意-不愿意。

**用法**:
```lisp
(dcl:end-dialog str-yes-no)
```

**参数**: 1 str-yes-no  : 字符串;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:end-dialog (str-yes-no / para)
  "完成创建DCL文件并关闭文件指针。
参数 str-Yes-No 字符串用 - 分隔成两部分，前面为accept,后面为 Cancel.如 是-否，愿意-不愿意。"
  ""
  (if (null str-yes-no)
      (setq str-yes-no "Yes-No"))
  (setq para (string:to-list str-yes-no "-"))
  (cond ((= (length para)
            0)
   (setq para (list "是" "否")))
  ((= (length para)
            1)
   (setq para (list (car para)
        "否"))))
  (write-line (strcat ":spacer{} : column {: row { fixed_width = true; alignment = centered; : retirement_button { label =\""
          (car para)"\";key=\"accept\";is_default=true;} :spacer{ width = 2; }:retirement_button {label= \""
          (cadr para) "\"; key =\"cancel\"; is_cancel = true;}}}}")
        dcl-fp)
  (close dcl-fp))
```
</details>

---

### dcl:error

**说明**: dcl 系列函数的错误处理，当dcl 运行出错后的一系列扫尾工作，防止对话框无法关闭而只能强行结束任务的情况。

**用法**:
```lisp
(dcl:error msg)
```

**参数**: 1 msg  : 提示信息;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:error (msg)
  "dcl 系列函数的错误处理，当dcl 运行出错后的一系列扫尾工作，防止对话框无法关闭而只能强行结束任务的情况。"
  (term_dialog)
  (if dcl-id (unload_dialog dcl-id))
  (if (= (quote file)
      (type dcl-fp))
    (close dcl-fp))
  (if (and (= (quote str)
        (type dcl-tmp))
      (findfile dcl-tmp))
    (vl-file-delete dcl-tmp))
  (@:*error* msg))
```
</details>

---

### dcl:hr

**说明**: DCL 水平线,粗度 size 值 推荐为 0.08(一个像素),0.17(两个像素).

**用法**:
```lisp
(dcl:hr size)
```

**参数**: 1 size  : 未识别定义;

**返回值**: dcl格式字符串

**示例**:
```lisp
(dcl:hr 0.08)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:hr (size / color)
  "DCL 水平线,粗度 size 值 推荐为 0.08(一个像素),0.17(两个像素)."
  "dcl格式字符串"
  "(dcl:hr 0.08)"
  (or (setq color theme:bg-color)
    (setq color 152))
  (write-line (strcat ":image{ height="
      (rtos size 2 2)
      "; color="
      (itoa color)
      "; fixed_height=true;vertical_margin=none;}")
    dcl-fp))
```
</details>

---

### dcl:image-button

**说明**: dcl 图像按钮。

**用法**:
```lisp
(dcl:image-button key width height style)
```

**参数**: 1 key  : 键，关键字;2 width  : 未识别定义;3 height  : 未识别定义;4 style  : 未识别定义;

**示例**:
```lisp
(dcl:image-button "btn1" 30 60)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:image-button (key width height style)
  "dcl 图像按钮。"
  ""
  "(dcl:image-button \"btn1\"
    30 60)"
  (set (read (strcat "cb-"
        key))
    (eval (read (strcat "(lambda()(alert (strcat \"需要定义回调函数 (cb-"
                  key ")\")))"))))
  (write-line (strcat ":image_button{key=\""
      key "\";width="
      (rtos width 2)
      ";height="
      (rtos height 2)
      ";color=152;"
      "action=\"(cb-"
        key ")\";"
      (dcl:lst2dcl style)
      "}")
    dcl-fp))
```
</details>

---

### dcl:img

**说明**: dcl 图像控件。

**用法**:
```lisp
(dcl:img key width height)
```

**参数**: 1 key  : 键，关键字;2 width  : 未识别定义;3 height  : 未识别定义;

**示例**:
```lisp
(dcl:img "img1 10 5)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:img (key width height)
  "dcl 图像控件。"
  ""
  "(dcl:img \"img1 10 5)"
  (write-line (strcat ":image{key=\""
      key "\";width="
      (rtos width 2)
      ";height="
      (rtos height 2)
      ";color=152;}")
    dcl-fp))
```
</details>

---

### dcl:input

**说明**: dcl 输入框。

**用法**:
```lisp
(dcl:input key label default style)
```

**参数**: 1 key  : 键，关键字;2 label  : 标签;3 default  : 未识别定义;4 style  : 未识别定义;

**示例**:
```lisp
(dcl:input "in1" "label" "3" "")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:input (key label default style)
  "dcl 输入框。"
  ""
  "(dcl:input \"in1\"
    \"label\"
    \"3\"
    \"\")"
  (write-line (strcat ": edit_box{key=\""
      key "\";"
      "label=\""
      label "\";"
      "value=\""
      default "\";"
      (dcl:lst2dcl style)
      "}")
    dcl-fp))
```
</details>

---

### dcl:lst2dcl

**说明**: 将 lst 格式的DCL 描述表达式转为 DCL 格式. lst 格式当为点对时，表示为属性值对，当为列表时，第一个元素为 Tile 名。

**用法**:
```lisp
(dcl:lst2dcl lst)
```

**参数**: 1 lst  : 列表;

**返回值**: String

**示例**:
```lisp
(dcl:lst2dcl '(button (key . btn)(label . BTN)(width . 20)(height . 3)) ;; => :button {key="btn";label="BTN";width=20;height=3;}
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:lst2dcl (lst)
  "将 lst 格式的DCL 描述表达式转为 DCL 格式. lst 格式说明：当为点对时，表示为属性值对，当为列表时，第一个元素为 Tile 名。"
  "String"
  "(dcl:lst2dcl '(button (key . btn)(label . BTN)(width . 20)(height . 3))
    ;; => :button {key=\"btn\";label=\"BTN\";width=20;height=3;}"
    (defun handle-value (sym)
      (cond ((p:intp sym)
          (itoa sym))
        ((numberp sym)
          (rtos sym 2 3))
        ((p:stringp sym)
          (strcat "\""
            sym "\""))
        ((member sym (quote (true false)))
          (vl-symbol-name sym))
        (t (strcat "\""
            (vl-symbol-name sym)
            "\""))))
    (if (p:stringp lst)
      lst (cond ((null lst)
          "")
        ((p:dotpairp lst)
          (strcat (vl-symbol-name (car lst))
            "="
            (handle-value (cdr lst))
            ";"))
        ((listp lst)
          (cond ((atom (car lst))
              (strcat ":"
                (vl-string-trim ":"
                  (vl-symbol-name (car lst)))
                "{"
                (dcl:lst2dcl (cdr lst))
                "}"))
            (t (string:from-lst (mapcar (quote dcl:lst2dcl)
                  lst)
                "")))))))
```
</details>

---

### dcl:mtext

**说明**: 带条格的多行文本。对赋传下的一段字符串自行换行处理。

**用法**:
```lisp
(dcl:mtext key rows width)
```

**参数**: 1 key  : 键，关键字;2 rows  : 未识别定义;3 width  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:mtext (key rows width / color)
  "带条格的多行文本。对赋传下的一段字符串自行换行处理。"
  (or (setq color theme:bg-color)
    (setq color 152))
  (setq i% 0)
  (write-line (strcat ":column{"
      ":image{ height=0.17; color="
      (itoa color)
      "; fixed_height=true;vertical_margin=none;}"
      ":image{ height=0.08; color="
      (itoa color)
      "; fixed_height=true;}")
    dcl-fp)
  (repeat rows (write-line (strcat ":text{key=\""
        key (itoa (1+ i%))
        "\";value=\"\";width="
        (itoa width)
        ";height=1.3;}"
        ":image{ height=0.08; color="
        (itoa color)
        "; fixed_height=true;}")
      dcl-fp)
    (setq i% (1+ i%)))
  (write-line (strcat ":image{ height=0.17; color="
      (itoa color)
      "; fixed_height=true;vertical_margin=none;}}:spacer{}")
    dcl-fp))
```
</details>

---

### dcl:new

**说明**: 载入DCL，并创建对话框名为 name 的对象。

**用法**:
```lisp
(dcl:new name)
```

**参数**: 1 name  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:new (name)
  "载入DCL，并创建对话框名为 name 的对象。"
  ""
  (setq dcl-id (load_dialog dcl-tmp))
  (if (not (new_dialog name dcl-id))
    (progn (princ "创建对话框失败，可能是太大了")
      (exit)))
  (action_tile "accept"
    "(dcl:accept)"))
```
</details>

---

### dcl:paging

**说明**: DCL 分页模块，key为 prev, next, curr_total. 参数 h-or-v : t 为 水平，nil 为竖向.Model部外部变量说明: curr-page:当前页号, total-page :总页数。Control部回调函数定义: (cb-flush-page) 页面更新。Init部: (paging-init)初始化按钮使能

**用法**:
```lisp
(dcl:paging h-or-v)
```

**参数**: 1 h-or-v  : 未识别定义;

**返回值**: dcl格式字符串。

**示例**:
```lisp
(dcl:paging t)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:paging (h-or-v / *error*)
  "DCL 分页模块，key为 prev, next, curr_total. \n参数 h-or-v : t 为 水平，nil 为竖向.\nModel部外部变量说明: curr-page:当前页号, total-page :总页数。\nControl部回调函数定义: (cb-flush-page)
  页面更新。\nInit部: (paging-init)初始化按钮使能"
  "dcl格式字符串。"
  "(dcl:paging t)"
  (defun *error* (msg)
    (if (/= (quote file)
        (type dcl-fp))
      (alert "请先运行 dcl:dialog ."))
    (@:*error* msg))
  (defun cb-flush-page nil "一般函数，需根据功能要求重新定义."
    (alert "请定义回调函数 (cb-flush-page)"))
  (defun paging-init nil (if (= 0 curr-page)
      (mode_tile "prev"
        1)
      (mode_tile "prev"
        0))
    (if (= (1- total-page)
        curr-page)
      (mode_tile "next"
        1)
      (mode_tile "next"
        0))
    (set_tile "curr_total"
      (strcat (itoa (1+ curr-page))
        "/"
        (itoa total-page))))
  (defun cb-page-up nil (setq curr-page (1- curr-page))
    (if (< curr-page 0)
      (setq curr-page 0))
    (if (= 0 curr-page)
      (mode_tile "prev"
        1)
      (mode_tile "prev"
        0))
    (if (= (1- total-page)
        curr-page)
      (mode_tile "next"
        1)
      (mode_tile "next"
        0))
    (set_tile "curr_total"
      (strcat (itoa (1+ curr-page))
        "/"
        (itoa total-page)))
    (cb-flush-page))
  (defun cb-page-down nil (setq curr-page (1+ curr-page))
    (if (> curr-page total-page)
      (setq curr-page total-page))
    (if (= 0 curr-page)
      (mode_tile "prev"
        1)
      (mode_tile "prev"
        0))
    (if (= (1- total-page)
        curr-page)
      (mode_tile "next"
        1)
      (mode_tile "next"
        0))
    (set_tile "curr_total"
      (strcat (itoa (1+ curr-page))
        "/"
        (itoa total-page)))
    (cb-flush-page))
  (write-line (strcat (if h-or-v ":row"
        ":column")
      "{alignment=centered;children_alignment=centered;"
      ":button{action=\"(cb-page-up)\";label=\"(<)Prev\";mnemonic=\"<\";key=\"prev\";is_enabled=false;alignment=centered;}"
      "spacer_0;"
      ":text_part{key=\"curr_total\"; value=\"\";alignment=centered;width=10;}"
      ":button{action=\"(cb-page-down)\";label=\"Next(>)\";mnemonic=\">\";key=\"next\";is_enabled=false;alignment=centered;}}")
    dcl-fp))
```
</details>

---

### dcl:password

**说明**: dcl 输入框。

**用法**:
```lisp
(dcl:password key label default style)
```

**参数**: 1 key  : 键，关键字;2 label  : 标签;3 default  : 未识别定义;4 style  : 未识别定义;

**示例**:
```lisp
(dcl:password "pw1" "label" "abc" "")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:password (key label default style)
  "dcl 输入框。"
  ""
  "(dcl:password \"pw1\"
    \"label\"
    \"abc\"
    \"\")"
  (write-line (strcat ": edit_box{key=\""
      key "\";"
      "label=\""
      label "\";"
      "value=\""
      default "\";password_char=\"*\";"
      (dcl:lst2dcl style)
      "}")
    dcl-fp))
```
</details>

---

### dcl:progressbar

**说明**: dcl 进度条。

**用法**:
```lisp
(dcl:progressbar key style show-txt?)
```

**参数**: 1 key  : 键，关键字;2 style  : 未识别定义;3 show-txt?  : 未识别定义;

**示例**:
```lisp
(dcl:progress-bar "pbar1" "" t)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:progressbar (key style show-txt? / color)
  "dcl 进度条。"
  ""
  "(dcl:progress-bar \"pbar1\"
    \"\"
    t)"
  (or (setq color theme:bg-color)
    (setq color 152))
  (write-line (strcat (if show-txt? ":row{"
        "")
      ":image{key=\""
      key "\";height=0.3;color=253;fixed_height=true;vertical_margin=none;"
      style "}"
      (if show-txt? (strcat ":text{key=\""
          key "txt\";width=4;fixed_width=true;}}")
        ""))
    dcl-fp))
```
</details>

---

### dcl:scrollbar

**用法**:
```lisp
(dcl:scrollbar key style h-or-v)
```

**参数**: 1 key  : 键，关键字;2 style  : 未识别定义;3 h-or-v  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:scrollbar (key style h-or-v)
  (set (read (strcat "cb-"
        key "scrollbar"))
    (lambda (key)
      "滚动条事件的回调函数"
      (alert (strcat "请定义回调函数 (cb-"
            key "scrollbar)"))))
  (if (eval (read (strcat key "total-page")))
    (write-line (strcat ":slider{action=\"(cb-"
          key "scrollbar \\\""
          key "\\\")\";key=\""
        key "\";layout="
        (if h-or-v "horizontal"
          "vertical")
        ";value="
        (itoa (1- (eval (read (strcat key "total-page")))))
        ";max_value="
        (itoa (1- (eval (read (strcat key "total-page")))))
        ";min_value=0;small_increment=1;}")
      dcl-fp)
    (alert (strcat "请在Model部定义 "
        key "total-page 变量。"))))
```
</details>

---

### dcl:set-mtext

**说明**: 给 mtext 控件赋值，自动换行。当 value 的长度大于 mtext 可以容纳的长度时，结尾加 ... 号。

**用法**:
```lisp
(dcl:set-mtext key value)
```

**参数**: 1 key  : 键，关键字;2 value  : 值;

**示例**:
```lisp
(dcl:set-mtext "mt" "给 mtext 控件赋值，自动换行。当 value 的长度大于 mtext 可以容纳的长度时，结尾加 ... 号。")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:set-mtext (key value / i% rows width)
  "给 mtext 控件赋值，自动换行。当 value 的长度大于 mtext 可以容纳的长度时，结尾加 ... 号。"
  ""
  "(dcl:set-mtext \"mt\"
    \"给 mtext 控件赋值，自动换行。当 value 的长度大于 mtext 可以容纳的长度时，结尾加 ... 号。\")"
  (setq i% 0)
  (while (get_tile (strcat key (itoa (setq i% (1+ i%))))))
  (setq rows (1- i%))
  (setq width (atoi (get_attr (strcat key "1")
        "width")))
  (setq i% 0)
  (setq lst-value% nil)
  (foreach c% (string:s2l-ansi value)
    (cond ((> i% rows)
        nil)
      ((= 10 c%)
        (set_tile (strcat key (itoa (setq i% (1+ i%))))
          (string:l2s-ansi (reverse lst-value%)))
        (setq lst-value% nil))
      ((> (string:bytelength (string:l2s-ansi (reverse (cdr lst-value%))))
          (- width 6))
        (set_tile (strcat key (itoa (setq i% (1+ i%))))
          (string:l2s-ansi (reverse lst-value%)))
        (setq lst-value% (cons c% nil)))
      (t (setq lst-value% (cons c% lst-value%)))))
  (if (and (> (length lst-value%)
        0)
      (<= i% rows))
    (set_tile (strcat key (itoa (setq i% (1+ i%))))
      (string:l2s-ansi (reverse lst-value%))))
  (while (get_tile (strcat key (itoa (setq i% (1+ i%)))))
    (set_tile (strcat key (itoa i%))
      "")))
```
</details>

---

### dcl:set-progressbar

**说明**: 设置 dcl 进度条的值。

**用法**:
```lisp
(dcl:set-progressbar key value)
```

**参数**: 1 key  : 键，关键字;2 value  : 值;

**示例**:
```lisp
(dcl:set-progressbar "pbar1" 0.5)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:set-progressbar (key value / w h color)
  "设置 dcl 进度条的值。"
  ""
  "(dcl:set-progressbar \"pbar1\"
    0.5)"
  (or (setq color theme:bg-color)
    (setq color 152))
  (if (> value 1)
    (setq value 1)
    (if (< value 0)
      (setq value 0)))
  (setq w (dimx_tile key)
    h (dimy_tile key))
  (start_image key)
  (fill_image 0 0 w h 253)
  (fill_image 0 0 (fix (* value w))
    h color)
  (end_image)
  (set_tile (strcat key "txt")
    (strcat (itoa (fix (* value 100)))
      "%")))
```
</details>

---

### dcl:show

**说明**: 显示对话框

**用法**:
```lisp
(dcl:show )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:show (/ ret)
  "显示对话框"
  ""
  (setq ret (start_dialog))
  (unload_dialog dcl-id)
  (vl-file-delete dcl-tmp)
  ret)
```
</details>

---

### dcl:text

**说明**: dcl 输入框。

**用法**:
```lisp
(dcl:text key label style)
```

**参数**: 1 key  : 键，关键字;2 label  : 标签;3 style  : 未识别定义;

**示例**:
```lisp
(dcl:text "text1" "text string" "")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun dcl:text (key label style)
  "dcl 输入框。"
  ""
  "(dcl:text \"text1\"
    \"text string\"
    \"\")"
  (write-line (strcat ": text{key=\""
      key "\";"
      "label=\""
      label "\";"
      (dcl:lst2dcl style)
      "}")
    dcl-fp))
```
</details>

---

## demo

**模块说明**: 专用功能模块

### demo:demo

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun demo:demo (lst pt)
  "这是一个演示，没有实际功能，只用于测试"
  "String"
  "(demo:demo '(a b)
    '(0 0 0))"
  (alert "测试用例"))
```
</details>

---

## entity

**模块说明**: 图元创建、修改、查询操作

### entity:activedimstyle

**说明**: 激活指定的标注样式。dimname:标注样式名

**用法**:
```lisp
(entity:activedimstyle dimname)
```

**参数**: 1 dimname  : 未识别定义;

**示例**:
```lisp
(activedimstyle "40")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:activedimstyle (dimname / acaddocument acadobject currdimstyle mspace)
  "激活指定的标注样式。dimname:标注样式名"
  ""
  "(activedimstyle \"40\")"
  (vl-load-com)
  (setq entname (tblobjname "DIMSTYLE"
      dimname))
  (setq acadobject (vlax-get-acad-object)
    acaddocument (vla-get-activedocument acadobject)
    mspace (vla-get-modelspace acaddocument))
  (setq currdimstyle (vlax-ename->vla-object entname))
  (vla-put-activedimstyle acaddocument currdimstyle)
  (princ))
```
</details>

---

### entity:activelayer

**说明**: 设置指定层为当前层.  name:图层名

**用法**:
```lisp
(entity:activelayer name)
```

**参数**: 1 name  : 未识别定义;

**返回值**: 成功返回t，失败返回nil

**示例**:
```lisp
(entity:ActiveLayer "layer1")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:activelayer (name / iloc out)
  "设置指定层为当前层.  name:图层名"
  "成功返回t，失败返回nil"
  "(entity:ActiveLayer \"layer1\")"
  (if (and (tblsearch "layer"
        name)
      (setq iloc (vl-position name (entity:layers))))
    (progn (vla-put-activelayer (std:active-document)
        (vla-item (std:layers)
          iloc))
      t)
    nil))
```
</details>

---

### entity:add-entitys-to-block

**说明**: 添加选择集到块定义。

**用法**:
```lisp
(entity:add-entitys-to-block block ss)
```

**参数**: 1 block  : 未识别定义;2 ss  : 选择集;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:add-entitys-to-block (block ss / lst mat)
  "添加选择集到块定义。"
  (setq lst (pickset->vlalist ss)
    mat (entity:reference->definition block)
    mat (vlax-tmatrix (append (mapcar (quote append)
          (car mat)
          (mapcar (quote list)
            (cadr mat)))
        (quote ((0.0 0.0 0.0 1.0))))))
  (foreach obj lst (vla-transformby obj mat))
  (vla-copyobjects (std:active-document)
    (vla:objarray lst)
    (vla-item (vla-get-blocks (std:active-document))
      (vla-get-name block)))
  (foreach obj lst (vla-delete obj))
  (vla-regen (std:active-document)
    acallviewports))
```
</details>

---

### entity:addhatch

**说明**: 创建填充。outArray:外边界对象表，inArray:内边界对象表，name:充填名称

**用法**:
```lisp
(entity:addhatch outarray inarray name)
```

**参数**: 1 outarray  : 未识别定义;2 inarray  : 未识别定义;3 name  : 未识别定义;

**返回值**: 填充体对象

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:addhatch (outarray inarray name / hatchobj)
  "创建填充。outArray:外边界对象表，inArray:内边界对象表，name:充填名称"
  "填充体对象"
  (setq hatchobj (vla-addhatch (std:model-space)
      achatchpatterntypepredefined name :vlax-true))
  (vla-appendouterloop hatchobj (vla:objarray (mapcar (quote vlax-ename->vla-object)
        outarray)))
  (if inarray (vla-appendinnerloop hatchobj (vla:objarray (mapcar (quote vlax-ename->vla-object)
          inarray))))
  (vla-put-patternscale hatchobj 40)
  hatchobj)
```
</details>

---

### entity:addtext

**说明**: 生成一个TEXT实体,entity:make-text参数简化版

**用法**:
```lisp
(entity:addtext str pt zg ang dq)
```

**参数**: 1 str  : 字符串;2 pt  : 单个2D/3D坐标点;3 zg  : 未识别定义;4 ang  : 角度值;5 dq  : 未识别定义;

**返回值**: return:文字图元名

**示例**:
```lisp
example:(entity:addtext "文字" (getpoint) 3 0 11)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:addtext (str pt zg ang dq)
  "生成一个TEXT实体,entity:make-text参数简化版"
  "return:文字图元名"
  "example:(entity:addtext \"文字\"
    (getpoint)
    3 0 11)"
  (entity:make-text str pt zg ang 0.8 0 dq))
```
</details>

---

### entity:block

**说明**: 将选择集、图元表、对象表创建为块。

**用法**:
```lisp
(entity:block ss name insertionpoint)
```

**参数**: 1 ss  : 选择集;2 name  : 未识别定义;3 insertionpoint  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:block (ss name insertionpoint / block)
  "将选择集、图元表、对象表创建为块。"
  (cond ((p:ename-listp ss)
      (setq ss (vla:objarray (mapcar (quote vlax-ename->vla-object)
            ss))))
    ((p:vla-listp ss)
      (setq ss (vla:objarray ss)))
    ((p:picksetp ss)
      (setq ss (pickset:to-array ss))))
  (print (type ss))
  (setq block (vla-add (vla-get-blocks *doc*)
      (vlax-3d-point insertionpoint)
      name))
  (vla-copyobjects *doc* ss block)
  (if (vl-catch-all-error-p (setq return (vl-catch-all-apply (quote (lambda nil (foreach obj (vlax-safearray->list ss)
                (vla-delete obj)))))))
    (vl-catch-all-error-message return))
  block)
```
</details>

---

### entity:boundary

**说明**: 创建图元所在的围区的边界多段线.

**用法**:
```lisp
(entity:boundary ent)
```

**参数**: 1 ent  : 单个图元;

**返回值**: 边界图元或nil

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:boundary (ent / pt boundary vis)
  "创建图元所在的围区的边界多段线."
  "边界图元或nil"
  (setq vis (vla-get-visible (e2o ent)))
  (setq pt (apply (quote point:mid)
      (entity:getbox ent 0)))
  (vla-put-visible (e2o ent)
    :vlax-false)
  (setq boundary (bpoly pt))
  (vla-put-visible (e2o ent)
    vis)
  boundary)
```
</details>

---

### entity:change-ltype

**说明**: 改变对象线型参数:obj:对象strLtype:线型

**用法**:
```lisp
(entity:change-ltype obj strltype)
```

**参数**: 1 obj  : activeX 对象;2 strltype  : 字符串;

**返回值**: 成功返回T，失败返回nil

**示例**:
```lisp
(entity:change-Ltype cirobj "DASHED")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:change-ltype (obj strltype / entlist)
  "改变对象线型\n参数:\nobj:对象\nstrLtype:线型"
  "成功返回T，失败返回nil"
  "(entity:change-Ltype cirobj \"DASHED\")"
  (cond ((entity:ltype-exists strltype)
      (cond ((and (vlax-read-enabled-p obj)
            (vlax-write-enabled-p obj))
          (vla-put-linetype obj strltype)
          t)
        (t nil)))
    (t nil)))
```
</details>

---

### entity:change-textstyle

**说明**: 更改指定字体样式的字体参数:TextStyleName:字体样式名称FontName:字体名字BigFontName:大字体名字

**用法**:
```lisp
(entity:change-textstyle textstylename fontname bigfontname)
```

**参数**: 1 textstylename  : 未识别定义;2 fontname  : 未识别定义;3 bigfontname  : 未识别定义;

**返回值**: 无

**示例**:
```lisp
(entity:ChangeTextStyle "STANDARD" "SIMfang.TTF" "")(entity:Change-TextStyle "STANDARD" "simplex.shx" "dayuxp.shx")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:change-textstyle (textstylename fontname bigfontname / txtstyle)
  "更改指定字体样式的字体\n参数:\nTextStyleName:字体样式名称\nFontName:字体名字\nBigFontName:大字体名字"
  "无"
  "(entity:ChangeTextStyle \"STANDARD\"
    \"SIMfang.TTF\"
    \"\")\n(entity:Change-TextStyle \"STANDARD\"
    \"simplex.shx\"
    \"dayuxp.shx\")"
  (setq txtstyle (vla-item (vla-get-textstyles (std:active-document))
      textstylename))
  (if (wcmatch (vl-filename-extension fontname)
      ".TTF,.ttf")
    (vla-put-fontfile txtstyle (strcat (getenv "Windir")
        "\\fonts\\"
        FONTNAME))
    (PROGN (vla-put-FontFile TXTSTYLE FONTNAME)
      (vla-put-BigFontFile TXTSTYLE BIGFONTNAME)))
  (vla-Regen (STD:ACTIVE-DOCUMENT)
    acAllViewports)
  (vlax-release-object TXTSTYLE)
  (PRINC))
```
</details>

---

### entity:check-error-codes

**说明**: 消除字体乱码，利用gbenor.shx gbcbig.shx参数:doc:当前活动文档

**用法**:
```lisp
(entity:check-error-codes doc)
```

**参数**: 1 doc  : 未识别定义;

**返回值**: 无

**示例**:
```lisp
(entity:Check-Error-Codes *DOC*)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:check-error-codes (doc)
  "消除字体乱码，利用gbenor.shx gbcbig.shx\n参数:\ndoc:当前活动文档"
  "无"
  "(entity:Check-Error-Codes *DOC*)"
  (vlax-for txtstyle (vla-get-textstyles doc)
    (if (findfile (vla-get-fontfile txtstyle))
      nil (vla-put-fontfile txtstyle "tssdeng.shx"))
    (if (findfile (vla-get-bigfontfile txtstyle))
      nil (vla-put-bigfontfile txtstyle "tssdchn.shx")))
  (princ))
```
</details>

---

### entity:deldxf

**说明**: 删除图元的某一组码，用于操作颜色等不是必段的组码。参数:ename:图元，选择集，图元列表code:组码或组码表

**用法**:
```lisp
(entity:deldxf ename code)
```

**参数**: 1 ename  : 单个图元;2 code  : 未识别定义;

**返回值**: 更新后的图元，选择集，图元列表

**示例**:
```lisp
(entity:deldxf (car (entsel))    62 )
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:deldxf (ename code / ent)
  "删除图元的某一组码，用于操作颜色等不是必段的组码。\n参数:\nename:图元，选择集，图元列表\ncode:组码或组码表\n"
  "更新后的图元，选择集，图元列表"
  "(entity:deldxf (car (entsel))\n    62 )"
  (cond ((p:enamep ename)
      (if (null (wcmatch (entity:getdxf ename 0)
            "TCH_*"))
        (progn (setq ent (entget ename))
          (if (and (listp code))
            (mapcar (quote (lambda (x y)
                  (entity:deldxf ename x)))
              code val)
            (progn (if (entity:getdxf ename code)
                (entmod (vl-remove (assoc code ent)
                    ent)))
              (entupd ename))))
        (@:log "WARR"
          "this function CANNOT support proxy entity.")))
    ((p:picksetp ename)
      (foreach s1 (pickset:to-list ename)
        (entity:deldxf s1 code)))
    ((p:ename-listp ename)
      (foreach s1 ename (entity:deldxf s1 code))))
  ename)
```
</details>

---

### entity:dimaligned

**说明**: 创建对齐标注

**用法**:
```lisp
(entity:dimaligned p1 p2 txtpt)
```

**参数**: 1 p1  : 未识别定义;2 p2  : 未识别定义;3 txtpt  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:dimaligned (p1 p2 txtpt)
  "创建对齐标注"
  (entmakex (list (quote (0 . "DIMENSION"))
      (quote (100 . "AcDbEntity"))
      (quote (100 . "AcDbDimension"))
      (cons 10 txtpt)
      (quote (70 . 33))
      (quote (1 . ""))
      (quote (100 . "AcDbAlignedDimension"))
      (cons 13 p1)
      (cons 14 p2))))
```
</details>

---

### entity:dimdiameter

**说明**: 生成直径标注

**用法**:
```lisp
(entity:dimdiameter pt1 pt2 pt-txt)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;2 pt2  : 单个2D/3D坐标点;3 pt-txt  : 单个2D/3D坐标点;

**返回值**: return:标注图元名

**示例**:
```lisp
example:(entity:dimdiameter (getpoint) (getpoint)(getpoint))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:dimdiameter (pt1 pt2 pt-txt)
  "生成直径标注"
  "return:标注图元名"
  "example:(entity:dimdiameter (getpoint)
    (getpoint)(getpoint))"
  (entmakex (list (quote (0 . "DIMENSION"))
      (quote (100 . "AcDbEntity"))
      (quote (100 . "AcDbDimension"))
      (cons 10 pt1)
      (cons 11 pt-txt)
      (quote (70 . 163))
      (quote (100 . "AcDbDiametricDimension"))
      (cons 15 pt2))))
```
</details>

---

### entity:dimhorizontal

**说明**: 生成水平标注

**用法**:
```lisp
(entity:dimhorizontal pt1 pt2 pt-txt)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;2 pt2  : 单个2D/3D坐标点;3 pt-txt  : 单个2D/3D坐标点;

**返回值**: return:标注图元名

**示例**:
```lisp
example:(entity:dimhorizontal (getpoint) (getpoint) (getpoint))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:dimhorizontal (pt1 pt2 pt-txt)
  "生成水平标注"
  "return:标注图元名"
  "example:(entity:dimhorizontal (getpoint)
    (getpoint)
    (getpoint))"
  (entmakex (list (quote (0 . "DIMENSION"))
      (quote (100 . "AcDbEntity"))
      (quote (100 . "AcDbDimension"))
      (cons 10 pt-txt)
      (quote (70 . 32))
      (quote (1 . ""))
      (quote (100 . "AcDbAlignedDimension"))
      (cons 13 pt1)
      (cons 14 pt2)
      (quote (100 . "AcDbRotatedDimension")))))
```
</details>

---

### entity:dimradius

**说明**: 生成半径标注

**用法**:
```lisp
(entity:dimradius pt-cen pt-r)
```

**参数**: 1 pt-cen  : 单个2D/3D坐标点;2 pt-r  : 单个2D/3D坐标点;

**返回值**: return:标注图元名

**示例**:
```lisp
example:(entity:dimradius (getpoint) (getpoint))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:dimradius (pt-cen pt-r)
  "生成半径标注"
  "return:标注图元名"
  "example:(entity:dimradius (getpoint)
    (getpoint))"
  (entmakex (list (quote (0 . "DIMENSION"))
      (quote (100 . "AcDbEntity"))
      (quote (100 . "AcDbDimension"))
      (cons 10 pt-cen)
      (quote (70 . 36))
      (quote (100 . "AcDbRadialDimension"))
      (cons 15 pt-r))))
```
</details>

---

### entity:dimvertical

**说明**: 创建竖向标注

**用法**:
```lisp
(entity:dimvertical p1 p2 txtpt)
```

**参数**: 1 p1  : 未识别定义;2 p2  : 未识别定义;3 txtpt  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:dimvertical (p1 p2 txtpt)
  "创建竖向标注"
  (entmakex (list (quote (0 . "DIMENSION"))
      (quote (100 . "AcDbEntity"))
      (quote (100 . "AcDbDimension"))
      (cons 10 txtpt)
      (quote (70 . 32))
      (quote (1 . ""))
      (quote (100 . "AcDbAlignedDimension"))
      (cons 13 p1)
      (cons 14 p2)
      (quote (50 . 1.5708))
      (quote (100 . "AcDbRotatedDimension")))))
```
</details>

---

### entity:fontstyle_set

**说明**: 验证字体样式是否存在，若不存在，则新建字体样式参数：st_name : 文字样式名h : 字高

**用法**:
```lisp
(entity:fontstyle_set st_name h)
```

**参数**: 1 st_name  : 未识别定义;2 h  : 未识别定义;

**示例**:
```lisp
(fontstyle_set "仿宋_GB2312" 0)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:fontstyle_set (st_name h / sty)
  "验证字体样式是否存在，若不存在，则新建字体样式\n参数：\nst_name : 文字样式名\nh : 字高"
  ""
  "(fontstyle_set \"仿宋_GB2312\"
    0)"
  (setq sty (tblobjname "style"
      st_name))
  (if (null sty)
    (progn (entmake (list (quote (0 . "STYLE"))
          (quote (100 . "AcDbSymbolTableRecord"))
          (quote (100 . "AcDbTextStyleTableRecord"))
          (cons 2 st_name)
          (quote (70 . 0))
          (cons 40 h)
          (cons 41 0.7)
          (quote (3 . ""))
          (quote (4 . "")))))))
```
</details>

---

### entity:get-color

**说明**: 获取图元的颜色，当颜色随层时，返回图层颜色。

**用法**:
```lisp
(entity:get-color ent)
```

**参数**: 1 ent  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:get-color (ent)
  "获取图元的颜色，当颜色随层时，返回图层颜色。"
  (if (entity:getdxf ent 62)
    (entity:getdxf ent 62)
    (cdr (assoc 62 (tblsearch "layer"
          (entity:getdxf ent 8))))))
```
</details>

---

### entity:get-layer

**说明**: 获取图元的图层名

**用法**:
```lisp
(entity:get-layer ent)
```

**参数**: 1 ent  : 单个图元;

**返回值**: String

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:get-layer (ent)
  "获取图元的图层名"
  "String"
  (entity:getdxf ent 8))
```
</details>

---

### entity:get-linetype

**说明**: 获取图元的线型，当线型随层时，返回图层线型。

**用法**:
```lisp
(entity:get-linetype ent)
```

**参数**: 1 ent  : 单个图元;

**返回值**: String

**示例**:
```lisp
(entity:get-linetype (car(entsel)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:get-linetype (ent)
  "获取图元的线型，当线型随层时，返回图层线型。"
  "String"
  "(entity:get-linetype (car(entsel)))"
  (if (entity:getdxf ent 6)
    (entity:getdxf ent 6)
    (cdr (assoc 6 (tblsearch "layer"
          (entity:getdxf ent 8))))))
```
</details>

---

### entity:get-truecolor

**说明**: 获取图元 RGB 真彩色

**用法**:
```lisp
(entity:get-truecolor ent)
```

**参数**: 1 ent  : 单个图元;

**返回值**: lst

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:get-truecolor (ent / obj-color)
  "获取图元 RGB 真彩色"
  "lst"
  (if (= (quote ename)
      (type ent))
    (setq ent (e2o ent)))
  (if (p:vlap ent)
    (progn (setq obj-color (vla-get-truecolor ent))
      (list (vla-get-red obj-color)
        (vla-get-green obj-color)
        (vla-get-blue obj-color)))
    (quote (0 0 0))))
```
</details>

---

### entity:getbox

**说明**: 图元的最小包围盒

**用法**:
```lisp
(entity:getbox ent offset)
```

**参数**: 1 ent  : 单个图元;2 offset  : 偏移量;

**返回值**: return:外框（偏移后）的左下，右上角点

**示例**:
```lisp
example:(entity:getbox (car(entsel)) 0.1)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:getbox (ent offset / lst obj p1 p2 p3 p4)
  "图元的最小包围盒"
  "return:外框（偏移后）的左下，右上角点"
  "example:(entity:getbox (car(entsel))
    0.1)"
  (if (= (quote pickset)
      (type ent))
    (pickset:getbox ent offset)
    (progn (setq obj (vlax-ename->vla-object ent))
      (vla-getboundingbox obj (quote p1)
        (quote p3))
      (setq p1 (vlax-safearray->list p1)
        p3 (vlax-safearray->list p3))
      (if (= "SPLINE"
          (cdr (assoc 0 (entget ent))))
        (progn (setq lst (mapcar (quote (lambda (a b)
                  (vlax-curve-getclosestpointtoprojection ent a b t)))
              (list p1 (list (car p1)
                  (cadr p3)
                  (caddr p1))
                p3 (list (car p3)
                  (cadr p1)
                  (caddr p1)))
              (quote ((1.0 0 0)
                  (0 -1.0 0)
                  (-1.0 0 0)
                  (0 1.0 0)))))
          (setq p1 (apply (quote mapcar)
              (cons (quote min)
                lst))
            p3 (apply (quote mapcar)
              (cons (quote max)
                lst)))))
      (if (or (not offset)
          (equal offset 0 0.0001))
        (list p1 p3)
        (list (list:- p1 (list offset offset 0))
          (list:+ p3 (list offset offset 0)))))))
```
</details>

---

### entity:getdxf

**说明**: 获取图元的组码值参数:ent:图元名或vla对象名i:组码或组码表

**用法**:
```lisp
(entity:getdxf ent i)
```

**参数**: 1 ent  : 单个图元;2 i  : 未识别定义;

**返回值**: 组码值或列表

**示例**:
```lisp
(entity:getdxf (car (entsel)) 10)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:getdxf (ent i / getdxf result)
  "获取图元的组码值\n参数:\nent:图元名或vla对象名\ni:组码或组码表"
  "组码值或列表"
  "(entity:getdxf (car (entsel))
    10)"
  (defun getdxf (ent i)
    (mapcar (quote cdr)
      (vl-remove-if-not (quote (lambda (x)
            (= (car x)
              i)))
        ent)))
  (cond ((p:vlap ent)
      (setq ent (entget (vlax-vla-object->ename ent)
          (quote ("*")))))
    ((p:enamep ent)
      (setq ent (entget ent (quote ("*"))))))
  (cond ((atom i)
      (setq result (getdxf ent i)))
    ((listp i)
      (setq result (apply (quote append)
          (mapcar (quote (lambda (x)
                (getdxf ent x)))
            i)))))
  (if (= 1 (length result))
    (car result)
    result))
```
</details>

---

### entity:gettable

**用法**:
```lisp
(entity:gettable s)
```

**参数**: 1 s  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:gettable (s / d r)
  (while (setq d (tblnext s (null d)))
    (setq r (cons (cdr (assoc 2 d))
        r)))
  (reverse r))
```
</details>

---

### entity:gettextbox

**说明**: 获取单行文本包围框

**用法**:
```lisp
(entity:gettextbox ent-text offset)
```

**参数**: 1 ent-text  : 单个图元;2 offset  : 偏移量;

**返回值**: return:文字外框（偏移后）的四个角点（左下，右下，右上，左上

**示例**:
```lisp
example:(entity:getTextBox (car(entsel)) 2)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:gettextbox (ent-text offset / pt1 pt2 pts)
  "获取单行文本包围框"
  "return:文字外框（偏移后）的四个角点（左下，右下，右上，左上"
  "example:(entity:getTextBox (car(entsel))
    2)"
  (setq pts (textbox (entget ent-text)))
  (if offset (entity:rec-2pt->4pt (list:- (car pts)
        (list offset offset 0))
      (list:+ (cadr pts)
        (list offset offset 0)))
    pts))
```
</details>

---

### entity:group

**说明**: 将实体集编组

**用法**:
```lisp
(entity:group lst name)
```

**参数**: 1 lst  : 列表;2 name  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:group (lst name / groupobj)
  "将实体集编组"
  (setq groupobj (vla-add (vla-get-groups (vla-get-activedocument (vlax-get-acad-object)))
      name))
  (vla-appenditems groupobj (vla:list->array lst 9)))
```
</details>

---

### entity:layers

**说明**: 获取图层列表

**用法**:
```lisp
(entity:layers )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:layers nil "获取图层列表"
  (entity:listcollection (std:layers)))
```
</details>

---

### entity:line

**说明**: 在模型空间画直线

**用法**:
```lisp
(entity:line start end)
```

**参数**: 1 start  : 未识别定义;2 end  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:line (start end)
  "在模型空间画直线"
  (vla-addline (std:model-space)
    (vlax-3d-point start)
    (vlax-3d-point end)))
```
</details>

---

### entity:linetypes

**用法**:
```lisp
(entity:linetypes )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:linetypes nil (entity:listcollection (std:linetypes)))
```
</details>

---

### entity:listcollection

**说明**: 列集合

**用法**:
```lisp
(entity:listcollection collection)
```

**参数**: 1 collection  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:listcollection (collection / out)
  "列集合"
  (vlax-for each collection (setq out (cons (vla-get-name each)
        out)))
  (reverse out))
```
</details>

---

### entity:ltype-exists

**说明**: 线型是否存在?参数:strLtype:线型名

**用法**:
```lisp
(entity:ltype-exists strltype)
```

**参数**: 1 strltype  : 字符串;

**返回值**: 成功返回t，失败返回nil

**示例**:
```lisp
(entity:Ltype-Exists "continuous")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:ltype-exists (strltype)
  "线型是否存在?\n参数:\nstrLtype:线型名"
  "成功返回t，失败返回nil"
  "(entity:Ltype-Exists \"continuous\")"
  (and (member (strcase strltype)
      (mapcar (quote strcase)
        (entity:linetypes)))))
```
</details>

---

### entity:make-arc

**说明**: 创建圆弧

**用法**:
```lisp
(entity:make-arc cen rad startpt endpt)
```

**参数**: 1 cen  : 未识别定义;2 rad  : 未识别定义;3 startpt  : 未识别定义;4 endpt  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-arc (cen rad startpt endpt)
  "创建圆弧"
  (entmakex (list (quote (0 . "ARC"))
      (quote (100 . "AcDbEntity"))
      (quote (100 . "AcDbCircle"))
      (quote (100 . "AcDbArc"))
      (cons 10 cen)
      (cons 40 rad)
      (cons 50 (if (listp startpt)
          (angle cen startpt)
          startpt))
      (cons 51 (if (p:listp endpt)
          (angle cen endpt)
          endpt)))))
```
</details>

---

### entity:make-arrow

**说明**: 生成箭头,一端宽，一端窄的多段线。参数:   startpt:箭头尖坐标   endpt:箭头尾坐标   width:箭头尾宽度返回值:  箭头图元名

**用法**:
```lisp
(entity:make-arrow startpt endpt width)
```

**参数**: 1 startpt  : 未识别定义;2 endpt  : 单个图元;3 width  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-arrow (startpt endpt width)
  "生成箭头,一端宽，一端窄的多段线。\n参数:\n   startpt:箭头尖坐标\n   endpt:箭头尾坐标\n   width:箭头尾宽度\n返回值:\n  箭头图元名\n"
  (entmakex (list (quote (0 . "LWPOLYLINE"))
      (quote (100 . "AcDbEntity"))
      (quote (100 . "AcDbPolyline"))
      (quote (90 . 2))
      (quote (70 . 0))
      (cons 10 startpt)
      (quote (40 . 0))
      (cons 41 width)
      (cons 10 endpt)
      (cons 40 width)
      (cons 41 width))))
```
</details>

---

### entity:make-circle

**说明**: 创建圆.如果圆心是点的列表或半径是数值的列表，可以同时创建多个圆

**用法**:
```lisp
(entity:make-circle pts-cen num-rad)
```

**参数**: 1 pts-cen  : 多个坐标点列表;2 num-rad  : 数值;

**返回值**: Ename

**示例**:
```lisp
(entity:make-circle (list (getpoint)(getpoint)) '(3 5))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-circle (pts-cen num-rad)
  "创建圆.如果圆心是点的列表或半径是数值的列表，可以同时创建多个圆"
  "Ename"
  "(entity:make-circle (list (getpoint)(getpoint))
    '(3 5))"
  (cond ((and (= (quote point)
          (type-of pts-cen))
        (numberp num-rad))
      (entmakex (list (quote (0 . "circle"))
          (quote (100 . "AcDbEntity"))
          (quote (100 . "AcDbCircle"))
          (cons 10 pts-cen)
          (cons 40 num-rad))))
    ((and (listp pts-cen)
        (apply (quote and)
          (mapcar (function (lambda (x)
                (= (quote point)
                  (type-of x))))
            pts-cen)))
      (mapcar (quote (lambda (pt)
            (entity:make-circle pt num-rad)))
        pts-cen))
    ((and (listp num-rad)
        (apply (quote and)
          (mapcar (quote numberp)
            num-rad)))
      (mapcar (quote (lambda (rad)
            (entity:make-circle pts-cen rad)))
        num-rad))
    (t (@:log "WARR"
        "参数错误,无创建圆")
      nil)))
```
</details>

---

### entity:make-dimstyle

**说明**: 创建标注样式,name:标注样式名

**用法**:
```lisp
(entity:make-dimstyle name)
```

**参数**: 1 name  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-dimstyle (name / my_dimasz my_dimaunit my_dimclrd my_dimclre my_dimclrt my_dimdli my_dimdsep my_dimexe my_dimexo my_dimlfac my_dimlwd my_dimlwe my_dimscale my_dimtad my_dimtih my_dimtix my_dimtofl my_dimtoh my_dimtxt my_dimzin)
  "创建标注样式,name:标注样式名"
  (entmake (list (quote (0 . "STYLE"))
      (quote (100 . "AcDbSymbolTableRecord"))
      (quote (100 . "AcDbTextStyleTableRecord"))
      (quote (2 . "标注"))
      (quote (70 . 0))
      (quote (40 . 0))
      (quote (41 . 0.8))
      (quote (50 . 0.0))
      (quote (71 . 0))
      (quote (42 . 2.5))
      (quote (3 . "SimSun.ttf"))
      (quote (4 . ""))))
  (entmake (list (quote (0 . "BLOCK"))
      (quote (100 . "AcDbEntity"))
      (quote (67 . 0))
      (quote (8 . "0"))
      (quote (100 . "AcDbBlockBegin"))
      (quote (70 . 0))
      (quote (10 0.0 0.0 0.0))
      (quote (2 . "_Oblique"))
      (quote (1 . ""))))
  (entmake (list (quote (0 . "LINE"))
      (quote (100 . "AcDbEntity"))
      (quote (67 . 0))
      (quote (8 . "0"))
      (quote (62 . 0))
      (quote (6 . "ByBlock"))
      (quote (370 . -2))
      (quote (100 . "AcDbLine"))
      (quote (10 -0.5 -0.5 0.0))
      (quote (11 0.5 0.5 0.0))
      (quote (210 0.0 0.0 1.0))))
  (entmake (list (quote (0 . "ENDBLK"))))
  (entupd (tblobjname "Block"
      "_Oblique"))
  (setq my_dimscale 40)
  (setq my_dimasz 2.5)
  (setq my_dimexo 1)
  (setq my_dimdli 3.75)
  (setq my_dimexe 1.25)
  (setq my_dimtxt 2.5)
  (setq my_dimlfac 1)
  (setq my_dimtih 0)
  (setq my_dimtoh 0)
  (setq my_dimtad 1)
  (setq my_dimzin 8)
  (setq my_dimtofl 1)
  (setq my_dimclrd 256)
  (setq my_dimclre 256)
  (setq my_dimclrt 256)
  (setq my_dimaunit 1)
  (setq my_dimdsep 46)
  (setq my_dimlwd -1)
  (setq my_dimlwe -1)
  (setq my_dimtix 1)
  (entmake (list (quote (0 . "DIMSTYLE"))
      (quote (100 . "AcDbSymbolTableRecord"))
      (quote (100 . "AcDbDimStyleTableRecord"))
      (cons 2 name)
      (quote (70 . 0))
      (quote (141 . 2.5))
      (quote (143 . 0.0393701))
      (quote (147 . 0.625))
      (quote (171 . 3))
      (quote (271 . 1))
      (quote (272 . 1))
      (quote (274 . 3))
      (quote (283 . 0))
      (quote (284 . 8))
      (cons 40 my_dimscale)
      (cons 41 my_dimasz)
      (cons 42 my_dimexo)
      (cons 43 my_dimdli)
      (cons 44 my_dimexe)
      (cons 140 my_dimtxt)
      (cons 144 my_dimlfac)
      (cons 73 my_dimtih)
      (cons 74 my_dimtoh)
      (cons 77 my_dimtad)
      (cons 78 my_dimzin)
      (cons 172 my_dimtofl)
      (cons 174 my_dimtix)
      (cons 176 my_dimclrd)
      (cons 177 my_dimclre)
      (cons 178 my_dimclrt)
      (cons 275 my_dimaunit)
      (cons 278 my_dimdsep)
      (cons 371 my_dimlwd)
      (cons 372 my_dimlwe)
      (cons 340 (tblobjname "STYLE"
          "标注"))
      (cons 342 (cdr (assoc 330 (entget (tblobjname "BLOCK"
                "_Oblique")))))))
  (entupd (tblobjname "Dimstyle"
      name))
  (princ))
```
</details>

---

### entity:make-layer

**说明**: 创建图层参数:strName:图层名intColor:图层颜色strLtype:图层线型booleCur:是否置为当前图层

**用法**:
```lisp
(entity:make-layer strname intcolor strltype boolecur)
```

**参数**: 1 strname  : 字符串;2 intcolor  : 整数;3 strltype  : 字符串;4 boolecur  : 未识别定义;

**返回值**: 成功返回图层名，失败返回nil

**示例**:
```lisp
(entity:make-layer "Layer1" 3 "DASHED" T)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-layer (strname intcolor strltype boolecur / iloc obj out)
  "创建图层\n参数:\nstrName:图层名\nintColor:图层颜色\nstrLtype:图层线型\nbooleCur:是否置为当前图层"
  "成功返回图层名，失败返回nil"
  "(entity:make-layer \"Layer1\"
    3 \"DASHED\"
    T)"
  (if (not (tblsearch "layer"
        strname))
    (progn (setq obj (vla-add (std:layers)
          strname))
      (setq iloc (vl-position strname (entity:layers)))
      (if (vlax-write-enabled-p obj)
        (progn (if intcolor (vla-put-color obj intcolor))
          (if strltype (entity:change-ltype obj strltype))))
      (if boolecur (vla-put-activelayer (std:active-document)
          (vla-item (std:layers)
            iloc)))
      strname)
    nil))
```
</details>

---

### entity:make-leader

**说明**: 创建无标记的箭头标注

**用法**:
```lisp
(entity:make-leader startpt endpt)
```

**参数**: 1 startpt  : 未识别定义;2 endpt  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-leader (startpt endpt)
  "创建无标记的箭头标注"
  (entmake (list (quote (0 . "leader"))
      (quote (100 . "AcDbEntity"))
      (quote (100 . "AcDbLeader"))
      (quote (71 . 1))
      (quote (72 . 0))
      (quote (73 . 3))
      (quote (74 . 1))
      (quote (75 . 0))
      (quote (40 . 0.0))
      (quote (41 . 0.0))
      (quote (76 . 2))
      (cons 10 startpt)
      (cons 10 endpt)
      (quote (211 1.0 0.0 0.0))
      (quote (210 0.0 0.0 1.0))
      (quote (212 0.0 0.0 0.0))
      (quote (213 0.0 0.0 0.0)))))
```
</details>

---

### entity:make-line

**说明**: 两点创建直线

**用法**:
```lisp
(entity:make-line startpt endpt)
```

**参数**: 1 startpt  : 未识别定义;2 endpt  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-line (startpt endpt)
  "两点创建直线"
  (entmakex (list (quote (0 . "LINE"))
      (quote (100 . "AcDbEntity"))
      (quote (100 . "AcDbLine"))
      (cons 10 startpt)
      (cons 11 endpt))))
```
</details>

---

### entity:make-lines

**说明**: 按多个点坐标创建连续直线

**用法**:
```lisp
(entity:make-lines pts)
```

**参数**: 1 pts  : 多个坐标点列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-lines (pts)
  "按多个点坐标创建连续直线"
  (mapcar (quote entity:make-line)
    (list:rtrim pts 1)
    (cdr pts)))
```
</details>

---

### entity:make-lwpline-bold

**说明**: 生成固定宽度的二维多段线.LWPOLYLINE参数:   plist:端点坐标点表，如：((x1 y1 z1)(x2 y2 z2)(x2 y2 z2))或((x1 y1)(x2 y2)(x2 y2))   convexity:各点与下一点的凸度(个数同坐标点表)，可为nil   elevation:标高   closed:是否闭合，1:闭合，0：不闭合

**用法**:
```lisp
(entity:make-lwpline-bold plist convexity elevation closed bold)
```

**参数**: 1 plist  : 未识别定义;2 convexity  : 未识别定义;3 elevation  : 未识别定义;4 closed  : 未识别定义;5 bold  : 未识别定义;

**返回值**: 返回值: 生成的多段线的图元名

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-lwpline-bold (plist convexity elevation closed bold / lst-dxf i)
  "生成固定宽度的二维多段线.LWPOLYLINE\n参数:\n   plist:端点坐标点表，如：((x1 y1 z1)(x2 y2 z2)(x2 y2 z2))或((x1 y1)(x2 y2)(x2 y2))\n   convexity:各点与下一点的凸度(个数同坐标点表)，可为nil\n   elevation:标高\n   closed:是否闭合，1:闭合，0：不闭合"
  "返回值: 生成的多段线的图元名"
  (setq lst-dxf (list (quote (0 . "LWPOLYLINE"))
      (quote (100 . "AcDbEntity"))
      (quote (62 . 1))
      (quote (370 . 30))
      (quote (100 . "AcDbPolyline"))
      (cons 90 (length plist))
      (if (= closed 1)
        (quote (70 . 1))
        (quote (70 . 0)))
      (cons 43 bold)
      (quote (38 . 0.0))
      (quote (39 . 0.0))))
  (setq i 0)
  (foreach x plist (setq lst-dxf (append lst-dxf (list (cons 10 x)
          (cons 40 bold)
          (cons 41 bold)
          (cons 42 (if (null convexity)
              0 (nth i convexity)))
          (cons 91 0))))
    (setq i (1+ i)))
  (entmake lst-dxf)
  (entlast))
```
</details>

---

### entity:make-lwpolyline

**说明**: 生成二维多段线.LWPOLYLINE参数:  plist:端点坐标点表，如：((x1 y1 z1) (x2 y2 z2) (x2 y2 z2))或((x1 y1) (x2 y2) (x2 y2))  convexity:各点与下一点的凸度(个数同坐标点表)，可为nil  linewidth : 宽度,当为数值时，为全局宽度，当为表时，为各段宽度。  closed:是否闭合，1:闭合，0：不闭合  elevation:标高

**用法**:
```lisp
(entity:make-lwpolyline plist convexity linewidth closed elevation)
```

**参数**: 1 plist  : 未识别定义;2 convexity  : 未识别定义;3 linewidth  : 未识别定义;4 closed  : 未识别定义;5 elevation  : 未识别定义;

**返回值**: 返回值: 生成多段线的图元名

**示例**:
```lisp
(entity:make-lwpolyline '((0 0) (2 2)) '(0.3 0) '((0.5 1.0)(0 0)) 0 nil)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-lwpolyline (plist convexity linewidth closed elevation / lst-dxf i)
  "生成二维多段线.LWPOLYLINE\n参数:\n  plist:端点坐标点表，如：((x1 y1 z1)
    (x2 y2 z2)
    (x2 y2 z2))或((x1 y1)
    (x2 y2)
    (x2 y2))\n  convexity:各点与下一点的凸度(个数同坐标点表)，可为nil\n  linewidth : 宽度,当为数值时，为全局宽度，当为表时，为各段宽度。\n  closed:是否闭合，1:闭合，0：不闭合\n  elevation:标高"
  "返回值: 生成多段线的图元名"
  "(entity:make-lwpolyline '((0 0)
      (2 2))
    '(0.3 0)
    '((0.5 1.0)(0 0))
    0 nil)"
  (setq lst-dxf (list (quote (0 . "LWPOLYLINE"))
      (quote (100 . "AcDbEntity"))
      (quote (62 . 1))
      (quote (370 . 30))
      (quote (100 . "AcDbPolyline"))
      (cons 90 (length plist))
      (if (= closed 1)
        (quote (70 . 1))
        (quote (70 . 0)))
      (if (numberp linewidth)
        (cons 43 linewidth))
      (quote (38 . 0.0))
      (quote (39 . 0.0))))
  (setq i 0)
  (foreach x plist (setq lst-dxf (append lst-dxf (list (cons 10 x)
          (if (listp linewidth)
            (cond ((numberp (nth i linewidth))
                (cons 40 (nth i linewidth)))
              ((and (listp (nth i linewidth))
                  (numberp (car (nth i linewidth))))
                (cons 40 (car (nth i linewidth))))
              (t (cons 40 0)))
            (cons 40 linewidth))
          (if (listp linewidth)
            (cond ((numberp (nth i linewidth))
                (cons 41 (nth i linewidth)))
              ((and (listp (nth i linewidth))
                  (numberp (cadr (nth i linewidth))))
                (cons 41 (cadr (nth i linewidth))))
              (t (cons 41 0)))
            (cons 41 linewidth))
          (cons 42 (if (null convexity)
              0 (nth i convexity)))
          (cons 91 0))))
    (setq i (1+ i)))
  (entmake (vl-remove nil lst-dxf))
  (entlast))
```
</details>

---

### entity:make-mtext

**说明**: 创建多行文本

**用法**:
```lisp
(entity:make-mtext str pt fontsize w h)
```

**参数**: 1 str  : 字符串;2 pt  : 单个2D/3D坐标点;3 fontsize  : 未识别定义;4 w  : 未识别定义;5 h  : 未识别定义;

**返回值**: ent

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-mtext (str pt fontsize w h / ent-mtext)
  "创建多行文本"
  "ent"
  (setq ent-mtext (entmakex (list (quote (0 . "MTEXT"))
        (quote (100 . "AcDbEntity"))
        (quote (67 . 0))
        (quote (100 . "AcDbMText"))
        (cons 10 pt)
        (cons 40 fontsize)
        (cons 41 w)
        (quote (46 . 0.0))
        (quote (71 . 1))
        (quote (72 . 5))
        (cons 1 "pre  mtext")
        (cons 7 (getvar "textstyle"))
        (quote (210 0.0 0.0 1.0))
        (quote (11 1.0 0.0 0.0))
        (cons 42 w)
        (cons 43 h)
        (quote (50 . 0.0))
        (quote (73 . 1))
        (quote (44 . 1.0)))))
  (vla-put-textstring (e2o ent-mtext)
    str)
  ent-mtext)
```
</details>

---

### entity:make-multileader

**说明**: 创建一个多重引线，

**用法**:
```lisp
(entity:make-multileader pts str)
```

**参数**: 1 pts  : 多个坐标点列表;2 str  : 字符串;

**返回值**: ename

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-multileader (pts str / mleader)
  "创建一个多重引线，"
  "ename"
  (setq pts (mapcar (quote point:2d->3d)
      pts))
  (setq points (vlax-make-safearray vlax-vbdouble (cons 0 (1- (* 3 (length pts))))))
  (vlax-safearray-fill points (apply (quote append)
      pts))
  (setq i 0)
  (setq mleader (vla-addmleader *ms* points i))
  (vla-put-textstring mleader str)
  (o2e mleader))
```
</details>

---

### entity:make-pline

**说明**: 生成二维多段线.POLYLINE参数:  plist:端点坐标点表，如：((x1 y1 z1) (x2 y2 z2) (x2 y2 z2))或((x1 y1) (x2 y2) (x2 y2))  convexity:各点与下一点的凸度(个数同坐标点表)，可为nil  elevation:标高  closed:是否闭合，1:闭合，0：不闭合

**用法**:
```lisp
(entity:make-pline plist convexity elevation closed)
```

**参数**: 1 plist  : 未识别定义;2 convexity  : 未识别定义;3 elevation  : 未识别定义;4 closed  : 未识别定义;

**返回值**: 返回值:  生成多段线的图元名

**示例**:
```lisp
示例:  (entity:make-pline '((0 0 0) (5000 0 0) (5000 5000 0) (0 5000 0)) '(-1.0 -0.5 0 -0.3) 100 1)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-pline (plist convexity elevation closed)
  "生成二维多段线.POLYLINE\n参数:\n  plist:端点坐标点表，如：((x1 y1 z1)
    (x2 y2 z2)
    (x2 y2 z2))或((x1 y1)
    (x2 y2)
    (x2 y2))\n  convexity:各点与下一点的凸度(个数同坐标点表)，可为nil\n  elevation:标高\n  closed:是否闭合，1:闭合，0：不闭合"
  "返回值:  生成多段线的图元名"
  "示例:  (entity:make-pline '((0 0 0)
      (5000 0 0)
      (5000 5000 0)
      (0 5000 0))
    '(-1.0 -0.5 0 -0.3)
    100 1)"
  (if (= closed 1)
    (entmake (list (quote (0 . "POLYLINE"))
        (quote (100 . "AcDbEntity"))
        (quote (67 . 0))
        (quote (410 . "Model"))
        (quote (8 . "0"))
        (quote (100 . "AcDb3dPolyline"))
        (quote (66 . 1))
        (quote (10 0.0 0.0 0.0))
        (quote (70 . 16))
        (quote (40 . 0.0))
        (quote (41 . 0.0))
        (quote (210 0.0 0.0 1.0))
        (quote (71 . 0))
        (quote (72 . 0))
        (quote (73 . 0))
        (quote (74 . 0))
        (quote (75 . 0))))
    (entmake (list (quote (0 . "POLYLINE"))
        (quote (66 . 1)))))
  (if convexity (mapcar (quote (lambda (x y)
          (entmake (list (cons 0 "VERTEX")
              (cons 10 x)
              (cons 42 y)))))
      plist convexity)
    (mapcar (quote (lambda (x)
          (entmake (list (cons 0 "VERTEX")
              (cons 10 x)))))
      plist))
  (entmake (quote ((0 . "SEQEND"))))
  (entlast))
```
</details>

---

### entity:make-point

**说明**: 根据参数坐标绘制一个点

**用法**:
```lisp
(entity:make-point pt)
```

**参数**: 1 pt  : 单个2D/3D坐标点;

**返回值**: ename or nil

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-point (pt)
  "根据参数坐标绘制一个点"
  "ename or nil"
  (entmakex (list (quote (0 . "POINT"))
      (quote (100 . "AcDbEntity"))
      (quote (67 . 0))
      (quote (100 . "AcDbPoint"))
      (cons 10 pt)
      (quote (210 0.0 0.0 1.0))
      (quote (50 . 0.0)))))
```
</details>

---

### entity:make-polyline

**说明**: 生成三维多段线.POLYLINE参数:  pts:端点坐标点表，如：((x1 y1 z1) (x2 y2 z2) (x2 y2 z2))  closed:是否闭合，1:闭合，0：不闭合

**用法**:
```lisp
(entity:make-polyline pts closed)
```

**参数**: 1 pts  : 多个坐标点列表;2 closed  : 未识别定义;

**返回值**: 生成多段线的图元名

**示例**:
```lisp
(entity:make-pline '((0 0 0) (5000 0 0) (5000 5000 0) (0 5000 0)) 1)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-polyline (pts closed / ent)
  "生成三维多段线.POLYLINE\n参数:\n  pts:端点坐标点表，如：((x1 y1 z1)
    (x2 y2 z2)
    (x2 y2 z2))\n  closed:是否闭合，1:闭合，0：不闭合"
  "生成多段线的图元名"
  "(entity:make-pline '((0 0 0)
      (5000 0 0)
      (5000 5000 0)
      (0 5000 0))
    1)"
  (if (= closed 1)
    (entmake (list (quote (0 . "POLYLINE"))
        (quote (66 . 1))
        (quote (10 0.0 0.0 0.0))
        (quote (70 . 9))))
    (entmake (list (quote (0 . "POLYLINE"))
        (quote (66 . 1))
        (quote (70 . 8)))))
  (if pts (mapcar (quote (lambda (x)
          (entmake (list (cons 0 "VERTEX")
              (cons 10 x)
              (cons 70 32)))))
      pts))
  (entmake (quote ((0 . "SEQEND"))))
  (entlast))
```
</details>

---

### entity:make-polyline-ax

**说明**: 根据点表生成polyline，三维多段线。参数:closed? T or nil.ActiveX 方法。

**用法**:
```lisp
(entity:make-polyline-ax pts-3d closed?)
```

**参数**: 1 pts-3d  : 多个坐标点列表;2 closed?  : 未识别定义;

**返回值**: 三维POLYLINE图元

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-polyline-ax (pts-3d closed? / acadobj doc modelspace pntlst2 points polyobj)
  "根据点表生成polyline，三维多段线。参数:closed? T or nil.ActiveX 方法。"
  "三维POLYLINE图元"
  (setq pntlst2 (quote nil))
  (if pts-3d (progn (foreach e pts-3d (setq pntlst2 (append pntlst2 e)))
      (setq acadobj (vlax-get-acad-object))
      (setq doc (vla-get-activedocument acadobj))
      (setq points (vlax-make-safearray vlax-vbdouble (cons 0 (- (length pntlst2)
              1))))
      (vlax-safearray-fill points pntlst2)
      (setq modelspace (vla-get-modelspace doc))
      (setq polyobj (vla-add3dpoly modelspace points))
      (if (= closed? t)
        (vla-put-closed (vlax-ename->vla-object (entlast))
          :vlax-true))))
  (entlast))
```
</details>

---

### entity:make-rectangle

**说明**: 创建矩形框(水平，竖直方向)

**用法**:
```lisp
(entity:make-rectangle pt1 pt2)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;2 pt2  : 单个2D/3D坐标点;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-rectangle (pt1 pt2)
  "创建矩形框(水平，竖直方向)"
  (entmake (list (quote (0 . "LWPOLYLINE"))
      (quote (100 . "AcDbEntity"))
      (quote (100 . "AcDbPolyline"))
      (quote (90 . 4))
      (quote (70 . 1))
      (if (caddr pt1)
        (cons 38 (caddr pt1))
        (cons 38 0))
      (cons 10 (list (car pt1)
          (cadr pt1)))
      (cons 10 (list (car pt2)
          (cadr pt1)))
      (cons 10 (list (car pt2)
          (cadr pt2)))
      (cons 10 (list (car pt1)
          (cadr pt2)))
      (cons 210 (quote (0 0 1)))))
  (entlast))
```
</details>

---

### entity:make-tag

**说明**: 生成一个标签

**用法**:
```lisp
(entity:make-tag pt name)
```

**参数**: 1 pt  : 单个2D/3D坐标点;2 name  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-tag (pt name)
  "生成一个标签"
  (entity:make-circle pt 10)
  (entity:maketext (vl-symbol-name name)
    pt 250 0 0.8 0 13))
```
</details>

---

### entity:make-text

**说明**: 生成一个TEXT实体,单行文本,参数fontsize: 字高ang: 角度kgb: 宽高比qx: 倾斜角dqys: 对齐方式，L 左 M 中 R 右，T 上 M 中 B 下。

**用法**:
```lisp
(entity:make-text str pt1 fontsize ang kgb qx dqys)
```

**参数**: 1 str  : 字符串;2 pt1  : 单个2D/3D坐标点;3 fontsize  : 未识别定义;4 ang  : 角度值;5 kgb  : 未识别定义;6 qx  : 未识别定义;7 dqys  : 未识别定义;

**返回值**: return:文字图元名

**示例**:
```lisp
example:(entity:make-text "文字" (getpoint) 3 0 0.8 0 "LB")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-text (str pt1 fontsize ang kgb qx dqys / y1 y2)
  "生成一个TEXT实体,单行文本,\n参数说明：\nfontsize: 字高\nang: 角度\nkgb: 宽高比\nqx: 倾斜角\ndqys: 对齐方式，L 左 M 中 R 右，T 上 M 中 B 下。\n"
  "return:文字图元名"
  "example:(entity:make-text \"文字\"
    (getpoint)
    3 0 0.8 0 \"LB\")"
  (if (= (quote str)
      (type dqys))
    (setq dqys (strcase dqys)))
  (cond ((or (= dqys 0)
        (= dqys "M"))
      (setq y1 (cons 72 4)
        y2 (cons 73 0)))
    ((or (= dqys 11)
        (= dqys "LT"))
      (setq y1 (cons 72 0)
        y2 (cons 73 3)))
    ((or (= dqys 12)
        (= dqys "LM"))
      (setq y1 (cons 72 0)
        y2 (cons 73 2)))
    ((or (= dqys 13)
        (= dqys "LB"))
      (setq y1 (cons 72 0)
        y2 (cons 73 1)))
    ((or (= dqys 21)
        (= dqys "MT"))
      (setq y1 (cons 72 1)
        y2 (cons 73 3)))
    ((or (= dqys 22)
        (= dqys "MM"))
      (setq y1 (cons 72 1)
        y2 (cons 73 2)))
    ((or (= dqys 23)
        (= dqys "MB"))
      (setq y1 (cons 72 1)
        y2 (cons 73 1)))
    ((or (= dqys 31)
        (= dqys "RT"))
      (setq y1 (cons 72 2)
        y2 (cons 73 3)))
    ((or (= dqys 32)
        (= dqys "RM"))
      (setq y1 (cons 72 2)
        y2 (cons 73 2)))
    ((or (= dqys 33)
        (= dqys "RB"))
      (setq y1 (cons 72 2)
        y2 (cons 73 1)))
    (t (setq y1 (cons 72 0)
        y2 (cons 73 3))))
  (entmakex (list (quote (0 . "TEXT"))
      (cons 10 pt1)
      (cons 1 str)
      (cons 40 fontsize)
      (cons 50 ang)
      (cons 41 kgb)
      (cons 51 qx)
      (quote (71 . 0))
      y1 y2 (cons 11 pt1))))
```
</details>

---

### entity:make-textstyle

**说明**: 创建文字样式。

**用法**:
```lisp
(entity:make-textstyle name)
```

**参数**: 1 name  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-textstyle (name / obj)
  "创建文字样式。"
  (setq obj (vla-add (vla-get-textstyles (vla-get-activedocument (vlax-get-acad-object)))
      name))
  (vla-setfont obj "宋体"
    :vlax-false :vlax-false 1 0)
  (vla-put-width obj 0.7)
  (princ))
```
</details>

---

### entity:make-xline

**说明**: 两点构造线。pt-base 基点，unit-vector 空间单位矢量，当为一个数值时，表示为弧度。

**用法**:
```lisp
(entity:make-xline pt-base unit-vector)
```

**参数**: 1 pt-base  : 单个2D/3D坐标点;2 unit-vector  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:make-xline (pt-base unit-vector)
  "两点构造线。pt-base 基点，unit-vector 空间单位矢量，当为一个数值时，表示为弧度。"
  ""
  ""
  (if (numberp unit-vector)
    (setq unit-vector (list (cos unit-vector)
        (sin unit-vector)
        0)))
  (entmakex (list (quote (0 . "XLINE"))
      (quote (100 . "AcDbEntity"))
      (quote (100 . "AcDbXline"))
      (cons 10 pt-base)
      (cons 11 unit-vector))))
```
</details>

---

### entity:offset

**说明**: 偏移对象

**用法**:
```lisp
(entity:offset obj dis)
```

**参数**: 1 obj  : activeX 对象;2 dis  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:offset (obj dis / offsetobj)
  "偏移对象"
  (if (p:enamep obj)
    (setq obj (vlax-ename->vla-object obj)))
  (setq offsetobj (vla-offset obj dis)))
```
</details>

---

### entity:onlockedlayer

**说明**: 解锁图元所在的图层

**用法**:
```lisp
(entity:onlockedlayer ename)
```

**参数**: 1 ename  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:onlockedlayer (ename / entlst)
  "解锁图元所在的图层"
  (setq entlst (tblsearch "LAYER"
      (cdr (assoc 8 (entget ename)))))
  (= 4 (logand 4 (cdr (assoc 70 entlst)))))
```
</details>

---

### entity:putdxf

**说明**: 更新图元的组码值参数:ename:图元，选择集，图元列表code:组码或组码表val:值或者值表

**用法**:
```lisp
(entity:putdxf ename code val)
```

**参数**: 1 ename  : 单个图元;2 code  : 未识别定义;3 val  : 值;

**返回值**: 更新后的图元，选择集，图元列表

**示例**:
```lisp
(entity:putdxf (car (entsel))    10 '(0 0 0))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:putdxf (ename code val / ent)
  "更新图元的组码值\n参数:\nename:图元，选择集，图元列表\ncode:组码或组码表\nval:值或者值表"
  "更新后的图元，选择集，图元列表"
  "(entity:putdxf (car (entsel))\n    10 '(0 0 0))"
  (cond ((p:enamep ename)
      (if (null (wcmatch (entity:getdxf ename 0)
            "TCH_*"))
        (progn (setq ent (entget ename))
          (if (and (listp code)
              (listp val))
            (mapcar (quote (lambda (x y)
                  (entity:putdxf ename x y)))
              code val)
            (if (null (entity:getdxf ename code))
              (entmod (append ent (list (cons code val))))
              (entmod (subst (cons code val)
                  (assoc code ent)
                  ent))))
          (entupd ename))
        (@:log "WARR"
          "this function CANNOT support proxy entity.")))
    ((p:picksetp ename)
      (foreach s1 (pickset:to-list ename)
        (entity:putdxf s1 code val)))
    ((p:ename-listp ename)
      (foreach s1 ename (entity:putdxf s1 code val))))
  ename)
```
</details>

---

### entity:readme

**说明**: 图元操作相关函数。

**用法**:
```lisp
(entity:readme )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:readme nil "图元操作相关函数。"
  (princ "图元操作相关函数。使用 (require 'entity:*)
    加载这些函数")
  (princ))
```
</details>

---

### entity:reference->definition

**说明**: 计算块参照与块定义的变换矩阵

**用法**:
```lisp
(entity:reference->definition ent)
```

**参数**: 1 ent  : 单个图元;

**返回值**: 返 回 值:3x3矩阵和向量组成的表

**示例**:
```lisp
示    例:(entity:Reference->Definition e)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:reference->definition (ent / a n)
  "计算块参照与块定义的变换矩阵"
  "返 回 值:3x3矩阵和向量组成的表"
  "示    例:(entity:Reference->Definition e)"
  (setq a (vla-get-rotation e)
    n (vla:getvalue (vla-get-normal e)))
  ((lambda (m)
      (list m (mapcar (quote -)
          (vla:-getvalue (vla-get-origin (vla-item (vla-get-blocks *doc*)
                (vla-get-name e))))
          (matrix:mxv m (trans (vla:-getvalue (vla-get-insertionpoint e))
              n 0)))))
    (matrix:mxm (list (list (/ 1.0 (vla-get-xscalefactor e))
          0.0 0.0)
        (list 0.0 (/ 1.0 (vla-get-yscalefactor e))
          0.0)
        (list 0.0 0.0 (/ 1.0 (vla-get-zscalefactor e))))
      (matrix:mxm (list (list (cos a)
            (sin (- a))
            0.0)
          (list (sin a)
            (cos a)
            0.0)
          (list 0.0 0.0 1.0))
        (mapcar (quote (lambda (e)
              (trans e n 0 t)))
          (quote ((1.0 0.0 0.0)
              (0.0 1.0 0.0)
              (0.0 0.0 1.0))))))))
```
</details>

---

### entity:set-visible

**说明**: 设置图元的可见性

**用法**:
```lisp
(entity:set-visible ent bool)
```

**参数**: 1 ent  : 单个图元;2 bool  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:set-visible (ent bool)
  "设置图元的可见性"
  ""
  (if bool (vla-put-visible (e2o ent)
      :vlax-true)
    (vla-put-visible (e2o ent)
      :vlax-false)))
```
</details>

---

### entity:spline

**用法**:
```lisp
(entity:spline pts)
```

**参数**: 1 pts  : 多个坐标点列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:spline (pts)
  (command "_SPLINE")
  (mapcar (quote command)
    pts)
  (command ""
    ""
    "")
  (entlast))
```
</details>

---

### entity:textstyles

**说明**: 文字样式集合

**用法**:
```lisp
(entity:textstyles )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:textstyles nil "文字样式集合"
  (entity:listcollection (std:textstyles)))
```
</details>

---

### entity:to-obj

**说明**: 图元类型转为ActiveX对象。简化函数 e2o

**用法**:
```lisp
(entity:to-obj en0)
```

**参数**: 1 en0  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun entity:to-obj (en0)
  "图元类型转为ActiveX对象。简化函数 e2o"
  (vlax-ename->vla-object en0))
```
</details>

---

## env

**模块说明**: 专用功能模块

### env:set-bg-color

**说明**: 设置绘图区背景色

**用法**:
```lisp
(env:set-bg-color col)
```

**参数**: 1 col  : 未明确定义;

**返回值**: 无

**示例**:
```lisp
(env:set-bg-color 55)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun env:set-bg-color (col / display)
    "设置绘图区背景色"
    "无"
    "(env:set-bg-color 55)"
    (setq display (vla-get-display (vla-get-preferences (vla-get-application (vlax-get-acad-object)))))
    (vla-put-graphicswinlayoutbackgrndcolor display (vlax-make-variant (+ col (* col 256)
                (* col 65536))
            vlax-vblong))
    (princ))
```
</details>

---

### env:set-cross-color

**用法**:
```lisp
(env:set-cross-color )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun env:set-cross-color (/ col display)
    (vla-put-modelcrosshaircolor display (vlax-make-variant (+ col (* col 256)
                (* col 65536))
            vlax-vblong)))
```
</details>

---

## example

**模块说明**: 专用功能模块

### example:cmd-reactor

**说明**: 命令反应器示例：当执行LINE PLINE ARC时，临时调整对象捕捉功能，完成后恢复原设置

**用法**:
```lisp
(example:cmd-reactor )
```

**参数**: 没有

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:cmd-reactor nil "命令反应器示例：当执行LINE PLINE ARC时，临时调整对象捕捉功能，完成后恢复原设置"
  ""
  ""
  ";;反转最近点捕捉命令，可加 ' 进行透明执行"
  (defun c:switchnea nil (setvar "osmode"
      (boole 6 (getvar "osmode")
        512))
    (if (= 0 (logand (getvar "osmode")
          512))
      (prompt "
        关闭最近点捕捉")
      (prompt "
        开启最近点捕捉"))
    (princ))
  ";;当执行命令 line pline arc 时，保存当前osmode变量"
  (defun react-start-cmd (param1 param2)
    (if (member (car param2)
        (quote ("LINE"
            "PLINE"
            "ARC")))
      (push-var "OSMODE"))
    (princ))
  ";; 当结束命令 line pline arc 时，恢复当前osmode变量"
  (defun react-end-cmd (param1 param2)
    (if (member (car param2)
        (quote ("LINE"
            "PLINE"
            "ARC")))
      (pop-var))
    (princ))
  ";; 定义命令反应器"
  (defun @::enable-tempvar-reactor nil (if (null at-tempvar-cmd-start)
      (setq at-tempvar-cmd-start (vlr-command-reactor nil (quote ((:vlr-commandwillstart . react-start-cmd)))))
      (vlr-add at-tempvar-cmd-start))
    (if (null at-tempvar-cmd-ended)
      (setq at-tempvar-cmd-ended (vlr-command-reactor nil (quote ((:vlr-commandended . react-end-cmd)
              (:vlr-commandcancelled . react-end-cmd)
              (:vlr-commandfailed . react-end-cmd)))))
      (vlr-add at-tempvar-cmd-ended)))
  (defun @::disable-tempvar-reactor nil (foreach x (list at-tempvar-cmd-start at-tempvar-cmd-ended)
      (if x (vlr-remove x))))
  (@::enable-tempvar-reactor))
```
</details>

---

### example:dbx

**说明**: 用DBX方式删除dwg文件中的图框签名和手写签名块

**用法**:
```lisp
(example:dbx )
```

**参数**: 没有一个

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:dbx nil "用DBX方式删除dwg文件中的图框签名和手写签名块"
  ""
  ""
  "修改以下配置信息==================================="
  (setq framename "图框块名")
  (setq signs (quote ("签名1"
        "签名2")))
  "
  =================================================="
  (defun dbx-clean-att (dwg-file / blk-obj blk3)
    "打开外部dwg的DBX对象. 假定文件为 D:/abc.dwg"
    (setq dwg-file (findfile dwg-file))
    (dbx:open dwg-file)
    "取dbx中的模型空间中的实体"
    (setq dbx-ms (vla-get-modelspace *dbx*))
    "取dbx块集中的第4个块定义对象（前几个是model-space paper-space之类的）"
    (setq n 0)
    (repeat (vla-get-count dbx-ms)
      (setq obj% (vla-item dbx-ms n))
      (print (vla-get-objectname obj%))
      (cond ((and (= (vla-get-objectname obj%)
              "AcDbBlockReference")
            (= (vla-get-hasattributes obj%)
              :vlax-true)
            (= (vla-get-effectivename obj%)
              framename))
          (block:set-attributes obj% (quote (("设计"
                  . "")
                ("校对"
                  . "")))))
        ((and (= (vla-get-objectname obj%)
              "AcDbBlockReference")
            (member (vla-get-effectivename obj%)
              signs))
          (vla-delete obj%)))
      (setq n (1+ n)))
    (setq tmp-file (strcat dwg-file ".b.dwg"))
    (vla-saveas *dbx* tmp-file)
    (dbx:release)
    (if (and (findfile tmp-file)
        (> (vl-file-size tmp-file)
          0))
      (progn (vl-file-delete dwg-file)
        (vl-file-rename tmp-file dwg-file))))
  (dbx-clean-att "D:/xxx.dwg"))
```
</details>

---

### example:dcl-cell

**说明**: MVCNIS 方法: 6 步进行动态 DCL 开发。表格示例。Model-View-Control-New-Init-Show.

**用法**:
```lisp
(example:dcl-cell )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:dcl-cell (/ curr-page total-page dcl-fp dcl-tmp)
  "MVCNIS 方法: 6 步进行动态 DCL 开发。表格示例。Model-View-Control-New-Init-Show."
  ""
  ""
  (require (quote dcl:*))
  (setq lst-cell (quote (("里程L"
          "高程H"
          "半径R")
        (0.0 218.0 0.0)
        (315.589 226.02 1000)
        (815.589 261.02 1000)
        (988.596 255.83 1000)
        (1407.44 260.01 1000)
        (1578.32 265.14 1000)
        (1768.39 264.57 1000)
        (2068.39 240.57 1000)
        (2168.39 237.57 1000)
        (2439.11 215.91 1000)
        (2527.98 213.9 10.0)
        (315.589 226.02 1000)
        (815.589 261.02 1000)
        (988.596 255.83 1000)
        (1407.44 260.01 1000)
        (1578.32 265.14 1000)
        (1768.39 264.57 1000)
        (2068.39 240.57 1000)
        (2168.39 237.57 1000)
        (2439.11 215.91 1000)
        (2527.98 213.9 10.0)
        (315.589 226.02 1000)
        (815.589 261.02 1000)
        (988.596 255.83 1000)
        (1407.44 260.01 1000)
        (1578.32 265.14 1000)
        (1768.39 264.57 1000)
        (2068.39 240.57 1000)
        (2168.39 237.57 1000)
        (2439.11 215.91 1000)
        (2527.98 213.9 10.0)
        (315.589 226.02 1000)
        (815.589 261.02 1000)
        (988.596 255.83 1000)
        (1407.44 260.01 1000)
        (1578.32 265.14 1000)
        (1768.39 264.57 1000)
        (2068.39 240.57 1000)
        (2168.39 237.57 1000)
        (2439.11 215.91 1000)
        (2527.98 213.9 10.0))))
  "1. Model 建立数据模型。"
  (setq accept-hook nil)
  (setq lst-cellraw lst-cell)
  (setq cell1tmp-data lst-cell)
  (setq cell1curr-page 0)
  "2. View 建立显示视图。"
  (dcl:dialog "example")
  (dcl:cell "cell1"
    9 (length (car cell1tmp-data))
    t t t t)
  (dcl:dialog-end-ok-cancel)
  "3. Control 创建控制流程"
  (dcl:new "example")
  "5. Init 初始化对话框"
  (set_tile "title"
    "Example 标题")
  (dcl:show-celldata "cell1")
  "6. Show"
  (dcl:show))
```
</details>

---

### example:dcl-dialog

**说明**: MVCNIS 法示例2: 6 步进行动态 DCL 开发。

**用法**:
```lisp
(example:dcl-dialog )
```

**参数**: 没有

**示例**:
```lisp
(example:dcl-dialog)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:dcl-dialog (/ *error* curr-page total-page dcl-fp dcl-tmp)
  "MVCNIS 法示例2: 6 步进行动态 DCL 开发。"
  ""
  "(example:dcl-dialog)"
  (require (quote dcl:*))
  "1. Model 建立数据模型。"
  (setq curr-page 0)
  (setq total-page 1)
  "2. View 建立显示视图。"
  (dcl:dialog "example")
  (progn (dcl:hr 0.08)
    (write-line ":text{key=\"num\";}"
      dcl-fp)
    (dcl:hr 0.08)
    (dcl:cell "cell"
      8 8 nil t nil nil)
    (dcl:hr 0.08)
    (dcl:paging t))
  (dcl:dialog-end-ok-cancel)
  "3. Control 创建控制流程"
  (defun cb-flush-page nil (set_tile "num"
      (strcat "\n         当前页面: "
        (itoa (1+ curr-page)))))
  "4. New 一个新对话框对象。"
  (dcl:new "example")
  "5. Init 初始化对话框"
  (set_tile "title"
    "Example 标题")
  (paging-init)
  (cb-flush-page)
  "6. Show dialog 显示并进行交互"
  (dcl:show)
  (princ))
```
</details>

---

### example:dcl-modify-title

**说明**: dcl改标题示例

**用法**:
```lisp
(example:dcl-modify-title )
```

**参数**: None

**示例**:
```lisp
(require 'example:dcl-modify-title)(example:dcl-modify-title)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:dcl-modify-title (/ *error* make-dcl dcl-tmp dcl-id)
  "dcl改标题示例"
  ""
  "(require 'example:dcl-modify-title)(example:dcl-modify-title)"
  (defun *error* (msg)
    (@:*error* msg))
  (defun make-dcl (/ lst_str str file f)
    (setq lst_str (quote (""
          "test:dialog {"
          "
             value=\"初始标题\";"
          "
             key = \"test\"
          ;"
          "
             :edit_box {"
          "
                 key = \"title\"
          ;"
          "
                 label = \"标题:\"
          ;"
          "
             }"
          "
             :button {"
          "
                 key = \"modify\"
          ;"
          "
                 label = \"更改\"
          ;"
          "
             }"
          "
             ok_cancel;"
          "}")))
    (setq file (vl-filename-mktemp "temp.dcl"))
    (setq f (open file "w"))
    (foreach str lst_str (princ "\n"
        f)
      (princ str f))
    (close f)
    file)
  (setq dcl-tmp (make-dcl))
  (setq dcl-id (load_dialog dcl-tmp))
  (if (not (new_dialog "test"
        dcl-id ""))
    (exit))
  (action_tile "modify"
    "(set_tile \"test\"
      (get_tile \"title\"))")
  (start_dialog)
  (unload_dialog dcl-id)
  (vl-file-delete dcl-tmp)
  (princ))
```
</details>

---

### example:dcl-mtext

**说明**: MVCNIS 法: 6 步进行动态 DCL 开发之多行文本

**用法**:
```lisp
(example:dcl-mtext )
```

**参数**: None

**示例**:
```lisp
(example:dcl-mtext)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:dcl-mtext (/ dcl-fp dcl-tmp)
  "MVCNIS 法: 6 步进行动态 DCL 开发之多行文本"
  ""
  "(example:dcl-mtext)"
  (require (quote dcl:*))
  "1. Model 建立数据模型。"
  "2. View 建立显示视图。"
  (dcl:dialog "example")
  (progn (dcl:mtext "mt"
      3 50)
    (dcl:begin-cluster "row"
      "")
    (progn (dcl:button "btn1"
        "按钮1"
        "")
      (dcl:button "btn2"
        "按钮2"
        "")
      (dcl:end-cluster)))
  (dcl:dialog-end-ok-cancel)
  "3. Control 创建控制流程"
  (defun cb-btn1 nil (dcl:set-mtext "mt"
      "按下了按钮1按下了按钮1按下了按钮1按下了按钮1按下了按钮1按下了按钮1按下了按钮1按下了按钮1"))
  (defun cb-btn2 nil (dcl:set-mtext "mt"
      "按下了按钮2按下了按钮2按下了按钮2按下了按钮2按下了按钮2按下了按钮2按下了按钮2按下了按钮2"))
  "4. New 一个新对话框对象。"
  (dcl:new "example")
  "5. Init 初始化对话框"
  (set_tile "title"
    "dcl-多行文本示例")
  (dcl:set-mtext "mt"
    "初始化多行文本内容。初始化多行文本内容。初始化多行文本内容。初始化多行文本内容。初始化多行文本内容。")
  "6. Show dialog 显示并进行交互"
  (dcl:show)
  (princ))
```
</details>

---

### example:dcl-mtext2

**说明**: MVCNIS 方法: 6 步进行动态 DCL 开发。Model-View-Control-New-Init-Show.

**用法**:
```lisp
(example:dcl-mtext2 )
```

**参数**: None

**示例**:
```lisp
(example:dcl-dialog)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:dcl-mtext2 (/ curr-page total-page dcl-fp dcl-tmp)
  "MVCNIS 方法: 6 步进行动态 DCL 开发。Model-View-Control-New-Init-Show."
  ""
  "(example:dcl-dialog)"
  (require (quote dcl:*))
  "1. Model 建立数据模型。"
  (setq curr-page 0)
  (setq total-page 5)
  "2. View 建立显示视图。"
  (dcl:dialog "example")
  (progn (dcl:begin-cluster "column"
      "")
    (progn (dcl:img "img1"
        20 10)
      (dcl:mtext "mtext"
        5 90)
      (dcl:begin-cluster "row"
        "")
      (progn (dcl:button "btn1"
          "Button1"
          "")
        (dcl:button "btn2"
          "Button2"
          "")
        (dcl:end-cluster))
      (dcl:end-cluster)))
  (dcl:dialog-end-ok-cancel)
  "3. Control 创建控制流程"
  (defun cb-flush-page nil (set_tile "num"
      (strcat "\n         当前页面: "
        (itoa (1+ curr-page)))))
  (defun cb-btn1 nil (dcl:set-mtext "mtext"
      "说明：字符串转字byte or word 整数值列表。n当小于128时，单字节，当两个连续的大于128时，双字节值。用于转换非英文字串时防止重码。n当AutoCAD2021且lispsys=1时，返回 unicode 码。说明：自动分段，按数字-字母-汉字自动断开字符串为字符串列表。不支持科学计数法的数字。说明：字符串转字byte or word 整数值列表。n当小于128时，单字节，当两个连续的大于128时，双字节值。用于转换非英文字串时防止重码。n当AutoCAD2021且lispsys=1时，返回 unicode 码。说明：自动分段，按数字-字母-汉字自动断开字符串为字符串列表。不支持科学计数法的数字。"))
  (defun cb-btn2 nil (dcl:set-mtext "mtext"
      "The button's label specifies text that appears inside the button. Buttons are appropriate for actions that are immediately visible to the user such as leaving the dialog box, or going into a subdialog box."))
  "4. New 一个新对话框对象。"
  (dcl:new "example")
  "5. Init 初始化对话框"
  (set_tile "title"
    "Example 标题")
  (cb-flush-page)
  (dcl:set-mtext "mtext"
    "动态多行文本。")
  (dcl:show)
  (princ))
```
</details>

---

### example:dcl-multirb

**说明**: 多行多列无线按钮的选取

**用法**:
```lisp
(example:dcl-multirb )
```

**参数**: 没有一个

**返回值**: 选中的按钮的key

**示例**:
```lisp
(example:dcl-multirb)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:dcl-multirb (/ dcl-fp rbn group-rb rb-value)
  "多行多列无线按钮的选取"
  "选中的按钮的key"
  "(example:dcl-multirb)"
  (setq rbn 0)
  (setq group-rb "slt")
  (setq rb-value nil)
  (dcl:dialog "buttons")
  (dcl:begin-cluster "radio_row"
    "")
  (repeat 3 (progn (dcl:begin-cluster "column"
        "")
      (repeat 3 (write-line (strcat ":radio_button{key=\""
            group-rb (itoa (setq rbn (1+ rbn)))
            "\"; label=\""
            "Select"
            (itoa rbn)
            "\";action=\"(cb-rb $key)\";}")
          dcl-fp))
      (dcl:end-cluster)))
  (dcl:end-cluster)
  (dcl:end-dialog str-yes-no)
  (defun cb-rb (key / i)
    (print key)
    (setq i 0)
    (repeat rbn (if (/= key (strcat group-rb (itoa (setq i (1+ i)))))
        (set_tile (strcat group-rb (itoa i))
          "0")
        (set_tile (strcat group-rb (itoa i))
          "1")))
    (setq rb-value key))
  (dcl:new "buttons")
  (dcl:show)
  rb-value)
```
</details>

---

### example:dcl-progressbar

**说明**: MVCNIS 法: 6 步进行动态 DCL 开发之进度条

**用法**:
```lisp
(example:dcl-progressbar )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:dcl-progressbar (/ dcl-fp dcl-tmp valuebar)
  "MVCNIS 法: 6 步进行动态 DCL 开发之进度条"
  ""
  ""
  (require (quote dcl:*))
  "1. Model 建立数据模型。"
  (setq value-bar 0.3)
  "2. View 建立显示视图。"
  (dcl:dialog "example")
  (progn (dcl:progressbar "pbar1"
      "width=30;fixed_width=true;"
      t)
    (dcl:begin-cluster "row"
      "")
    (progn (dcl:button "btn1"
        "进度-"
        "")
      (dcl:button "btn2"
        "进度+"
        "")
      (dcl:end-cluster)))
  (dcl:dialog-end-ok-cancel)
  "3. Control 创建控制流程"
  (defun chg-bar (step)
    (setq value-bar (+ value-bar step))
    (if (> value-bar 1)
      (setq value-bar 1)
      (if (< value-bar 0)
        (setq value-bar 0)))
    value-bar)
  (defun cb-btn1 nil (dcl:set-progressbar "pbar1"
      (chg-bar -0.1)))
  (defun cb-btn2 nil (dcl:set-progressbar "pbar1"
      (chg-bar 0.1)))
  "4. New 一个新对话框对象。"
  (dcl:new "example")
  "5. Init 初始化对话框"
  (set_tile "title"
    "dcl-进度条示例")
  (dcl:set-progressbar "pbar1"
    value-bar)
  "6. Show dialog 显示并进行交互"
  (dcl:show)
  (princ))
```
</details>

---

### example:dcl-progressbar2

**说明**: MVCNIS 法: 6 步进行动态 DCL 开发之进度条

**用法**:
```lisp
(example:dcl-progressbar2 )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:dcl-progressbar2 (/ dcl-fp dcl-tmp valuebar v1 v2 v3)
  "MVCNIS 法: 6 步进行动态 DCL 开发之进度条"
  ""
  ""
  (require (quote dcl:*))
  "1. Model 建立数据模型。"
  (setq value-bar 0.01 v1 0.01 v2 0.01 v3 0.01)
  "2. View 建立显示视图。"
  (dcl:dialog "example")
  (progn (dcl:progressbar "pbar0"
      "width=30;fixed_width=true;height=1;"
      t)
    (dcl:progressbar "pbar1"
      "width=30;fixed_width=true;"
      t)
    (dcl:progressbar "pbar2"
      "width=30;fixed_width=true;"
      t)
    (dcl:progressbar "pbar3"
      "width=30;fixed_width=true;"
      t)
    (dcl:mtext "mt"
      1 30)
    (dcl:begin-cluster "row"
      "")
    (progn (dcl:button "btn1"
        "清零"
        "")
      (dcl:button "btn2"
        "进度+"
        "")
      (dcl:end-cluster)))
  (dcl:dialog-end-ok-cancel)
  "3. Control 创建控制流程"
  (defun chg-bar (step)
    (dcl:set-mtext "mt"
      "
      执行第一阶段：")
    (while (and (< v1 1)
        (> v1 0))
      (setq v1 (+ v1 step))
      (if (> v1 1)
        (setq v1 1)
        (if (< v1 0)
          (setq v1 0)))
      (dcl:set-progressbar "pbar1"
        v1)
      (dcl:set-progressbar "pbar0"
        (/ (+ v1 v2 v3)
          3.0))
      (sleep 0.5))
    (dcl:set-mtext "mt"
      "
      执行第二阶段: ")
    (while (and (< v2 1)
        (> v2 0))
      (setq v2 (+ v2 (* 0.2 step)))
      (if (> v2 1)
        (setq v2 1)
        (if (< v2 0)
          (setq v2 0)))
      (dcl:set-progressbar "pbar2"
        v2)
      (dcl:set-progressbar "pbar0"
        (/ (+ v1 v2 v3)
          3.0))
      (sleep 0.2))
    (dcl:set-mtext "mt"
      "
      执行第三阶段: ")
    (while (and (< v3 1)
        (> v3 0))
      (setq v3 (+ v3 (* 0.3 step)))
      (if (> v3 1)
        (setq v3 1)
        (if (< v3 0)
          (setq v3 0)))
      (dcl:set-progressbar "pbar3"
        v3)
      (dcl:set-progressbar "pbar0"
        (/ (+ v1 v2 v3)
          3.0))
      (sleep 0.5))
    (dcl:set-mtext "mt"
      "
      执行完毕！！ "))
  (defun cb-btn1 nil (setq v1 0.001 v2 0.001 v3 0.001)
    (dcl:set-mtext "mt"
      "
      准备中 ... ")
    (mapcar (quote dcl:set-progressbar)
      (quote ("pbar0"
          "pbar1"
          "pbar2"
          "pbar3"))
      (quote (0.001 0.001 0.001 0.001))))
  (defun cb-btn2 nil (chg-bar 0.1))
  "4. New 一个新对话框对象。"
  (dcl:new "example")
  "5. Init 初始化对话框"
  (set_tile "title"
    "dcl-进度条示例")
  (cb-btn1)
  "6. Show dialog 显示并进行交互"
  (dcl:show)
  (princ))
```
</details>

---

### example:dcl-subdialog

**说明**: DCL多级对话框示例代码

**用法**:
```lisp
(example:dcl-subdialog )
```

**参数**: 没有一个

**示例**:
```lisp
(example:dcl-subdialog)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:dcl-subdialog (/ *error* curr-page total-page dcl-fp dcl-tmp cb-flush-page cb-img1 cb-img2 cb-img3)
  "DCL多级对话框示例代码"
  ""
  "(example:dcl-subdialog)"
  (require (quote dcl:*))
  (defun sub-dialog (m n / curr-page total-page dcl-fp dcl-tmp cb-flush-page page-init)
    "m n 表示图像的 行 列个数"
    "1. Model 建立数据模型。"
    (setq curr-page 0)
    (setq total-page 3)
    "2. View 建立显示视图。"
    (dcl:dialog "subimgs")
    (progn (dcl:hr 0.08)
      (write-line ":text{key=\"num\";}"
        dcl-fp)
      (dcl:hr 0.08)
      (setq i 0)
      (dcl:begin-cluster "column"
        "")
      (repeat m (dcl:begin-cluster "row"
          "")
        (repeat n (dcl:image-button (strcat "subimg"
              (itoa (setq i (1+ i))))
            20 10 nil))
        (dcl:end-cluster))
      (dcl:end-cluster)
      (dcl:hr 0.08)
      (dcl:paging t))
    (dcl:dialog-end-ok-cancel)
    "3. Control 创建控制流程"
    (defun cb-flush-page nil (set_tile "num"
        (strcat "当前页面: "
          (itoa (1+ curr-page)))))
    "4. New 一个新对话框对象。"
    (dcl:new "subimgs")
    "5. Init 初始化对话框"
    (set_tile "title"
      "子窗口")
    (paging-init)
    (cb-flush-page)
    "6. Show dialog 显示并进行交互"
    (dcl:show)
    (princ))
  "1. Model 建立数据模型。"
  (setq curr-page 0)
  (setq total-page 5)
  "2. View 建立显示视图。"
  (dcl:dialog "example")
  (progn (dcl:hr 0.08)
    (write-line ":text{key=\"num\";}"
      dcl-fp)
    (dcl:hr 0.08)
    (setq i 0)
    (dcl:begin-cluster "column"
      "")
    (repeat 4 (dcl:begin-cluster "row"
        "")
      (repeat 3 (dcl:image-button (strcat "img"
            (itoa (setq i (1+ i))))
          30 10 nil))
      (dcl:end-cluster))
    (dcl:end-cluster)
    (dcl:hr 0.08)
    (dcl:paging t))
  (dcl:dialog-end-ok-cancel)
  "3. Control 创建控制流程"
  (defun cb-flush-page nil (set_tile "num"
      (strcat "当前页面: "
        (itoa (1+ curr-page)))))
  (defun cb-img1 nil (sub-dialog 2 2))
  (defun cb-img2 nil (sub-dialog 3 3))
  (defun cb-img3 nil (sub-dialog 3 1))
  "4. New 一个新对话框对象。"
  (dcl:new "example")
  "5. Init 初始化对话框"
  (set_tile "title"
    "主窗口")
  (paging-init)
  (cb-flush-page)
  "6. Show dialog 显示并进行交互"
  (dcl:show)
  (princ))
```
</details>

---

### example:dcl1

**说明**: MVCNIS 法: 6 步进行动态 DCL 开发。

**用法**:
```lisp
(example:dcl1 )
```

**参数**: None

**示例**:
```lisp
(example:dcl-dialog)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:dcl1 (/ *error* dcl-fp dcl-tmp)
  "MVCNIS 法: 6 步进行动态 DCL 开发。"
  ""
  "(example:dcl-dialog)"
  (require (quote dcl:*))
  "1. Model 建立数据模型。"
  "2. View 建立显示视图。"
  (dcl:dialog "example")
  (progn (dcl:hr 0.08)
    (dcl:button "btn1"
      "按钮1"
      "")
    (dcl:hr 0.08)
    (dcl:button "btn2"
      "按钮2"
      "")
    (dcl:hr 0.08))
  (dcl:dialog-end-ok-cancel)
  "3. Control 创建控制流程"
  (defun cb-btn1 nil (alert "按下了按钮1"))
  (defun cb-btn2 nil (alert "按下了按钮2"))
  "4. New 一个新对话框对象。"
  (dcl:new "example")
  "5. Init 初始化对话框"
  (set_tile "title"
    "dcl示例1")
  "6. Show dialog 显示并进行交互"
  (dcl:show)
  (princ))
```
</details>

---

### example:error-exit

**说明**: 函数错误处理方案示例 *error* 定义私有函数，隔绝公共域 *error* ，使之不用重定义。参数 para .为 t 时执行正常流程，为 nil 时，执行错误处理流程。

**用法**:
```lisp
(example:error-exit para)
```

**参数**: 1 para  : 未识别定义;

**示例**:
```lisp
(example:error-exit nil)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:error-exit (para / *error*)
  "函数错误处理方案示例 *error* 定义私有函数，隔绝公共域 *error* ，使之不用重定义。\n参数 para .为 t 时执行正常流程，为 nil 时，执行错误处理流程。"
  ""
  "(example:error-exit nil)"
  (defun *error* (msg)
    "当异常退出时，正常流程没有关闭文件指针，在这里关闭"
    (if (= (quote file)
        (type fp))
      (progn (princ "关闭出错时的文件句本柄。\n")
        (close fp)))
    (princ "当异常退出时，设置的变量没有恢复，在这里恢复")
    (pop-var)
    (princ "显示恢复的变量值：")
    (princ (getvar "osmode"))
    (princ "\n")
    "以上为你专用的处理过程"
    "下面是 atlisp 常用错误处理过程"
    (@:*error* msg)
    (princ))
  "设置当前运行的函数名，当出错时给出信息。"
  (setq *funname* (quote example:error-exit))
  (princ "显示变量原值：")
  (princ (getvar "osmode"))
  (princ "\n")
  (push-var)
  "修改变量"
  (setvar "osmode"
    0)
  (princ "显示修改后的变量值：")
  (princ (getvar "osmode"))
  (princ "\n")
  (setq fp (open (findfile "acad.pgp")
      "r"))
  (read-line fp)
  (if (null para)
    (progn "这是一个未定义的函数。在这里异常退出."
      (mmma)))
  (princ "正常的流程,关闭文件句柄，恢复变量。")
  (close fp)
  (pop-var)
  (princ "显示恢复的变量值：")
  (princ (getvar "osmode"))
  (princ "\n")
  (princ))
```
</details>

---

### example:grread

**说明**: grread 编程示例，当按下键盘时，弹窗提示按的什么键，否则显示光标的坐标

**用法**:
```lisp
(example:grread )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:grread (/ flag r)
  "grread 编程示例，当按下键盘时，弹窗提示按的什么键，否则显示光标的坐标"
  (setq r 100.0)
  (setq flag t)
  (while flag (setq gr (grread t 16))
    "处理输入"
    (cond ((= 2 (car gr))
        "按下了键盘按键"
        (cond ((= "x"
              (chr (cadr gr)))
            (alert (strcat "按下了"
                (chr (cadr gr))))
            "设置条件退出循环"
            (setq flag nil))
          ((= "S"
              (strcase (chr (cadr gr))))
            "以下进行 按了 S的后处理程序"
            (setq r (cdr (assoc "半径"
                  (ui:input "设置"
                    (list (list "半径"
                        r)))))))
          ((= "C"
              (strcase (chr (cadr gr))))
            "以下进行 按了 C 的后处理程序-画圆"
            (entity:make-circle (cadr (grread t 0))
              r))
          (t (alert (vl-prin1-to-string gr)))))
      ((= 3 (car gr))
        "按下鼠标左键"
        (alert "click left"))
      ((or (= 25 (car gr))
          (= 11 (car gr)))
        "按下鼠标右键"
        (alert "click right"))
      ((= 5 (car gr))
        "移动鼠标"
        (princ "当前光标的坐标: ")
        (princ gr))
      (t "其它情况"
        (princ gr)))
    (princ "\n输入[ 退出x | 设置s | 画圆c ]"))
  (princ))
```
</details>

---

### example:multi-platform

**说明**: 多平台开发示例，如果你希望你的代码能在多种CAD平台（如autocad,浩辰CAD,中望CAD等）上运行。因为兼容性问题，需要处理一些特殊的代码段。以适应不同的平台

**用法**:
```lisp
(example:multi-platform )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:multi-platform (/ *error*)
  "多平台开发示例，如果你希望你的代码能在多种CAD平台（如autocad,浩辰CAD,中望CAD等）上运行。因为兼容性问题，需要处理一些特殊的代码段。以适应不同的平台"
  ""
  ""
  "atlisp 提供简单的解决方案，代码如下。"
  (cond (is-gcad "这里编写浩辰CAD的处理过程"
      (alert "这是浩辰CAD")
      (cond ((= "R21.2"
            (getvar "gcadver"))
          "浩辰不同版本的区别处理"
          (alert "这是浩辰CAD-v1"))
        ((= "R20.1"
            (getvar "gcadver"))
          "浩辰不同版本的区别处理"
          (alert "这是浩辰CAD-v2"))))
    (is-zwcad "这里编写中望CAD的处理过程"
      (alert "这是中望CAD"))
    (t "AutoCAD 的处理过程"
      (alert "这是AutoCAD"))))
```
</details>

---

### example:prev-var

**说明**: 上次输入的值作为默认值的示例

**用法**:
```lisp
(example:prev-var )
```

**参数**: 没有

**示例**:
```lisp
(example:prev-var)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:prev-var (/ k)
  "上次输入的值作为默认值的示例"
  ""
  "(example:prev-var)"
  (if (not (and prev-var (p:stringp prev-var)))
    (setq prev-var "D"))
  (initget "A W V D S B")
  (if (setq k (getkword (strcat "位置[A 在文字左边,W 在注解左边,V 在改模文字左边,D 在文字右边,S 手动添加,B 在模具BOM序号左边]<"
          (if (and prev-var (p:stringp prev-var))
            prev-var "D")
          ">:")))
    (setq prev-var k))
  prev-var)
```
</details>

---

### example:sort

**说明**: 排序示例.用于演示 vl-sort 自定义排序函数的代码。n1 n2 表示 列表内任意两个数，lambda 对这两个数进行比较。根据比较结果进行排序。

**用法**:
```lisp
(example:sort )
```

**参数**: None

**返回值**: 偶数从大到小，奇数从小到大,偶数在前

**示例**:
```lisp
(example:sort) => (8 6 4 2 0 1 3 5 7 9)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun example:sort nil "排序示例.用于演示 vl-sort 自定义排序函数的代码。n1 n2 表示 列表内任意两个数，lambda 对这两个数进行比较。根据比较结果进行排序。"
  "偶数从大到小，奇数从小到大,偶数在前"
  "(example:sort)
  => (8 6 4 2 0 1 3 5 7 9)"
  (vl-sort (list 1 2 4 5 6 7 8 9 0 3)
    (function (lambda (n1 n2)
        (if (= 0 (rem n1 2))
          (progn "前者为偶时"
            (if (= 0 (rem n2 2))
              (progn "如果后者也为偶，比大小"
                (> n1 n2))
              (progn "如果后者不为偶，保持"
                t)))
          (progn "前者为奇的情况"
            (if (/= 0 (rem n2 2))
              (progn "如果后者也为奇， 小数在前"
                (< n1 n2))
              (progn "后者不为奇，换位"
                nil))))))))
```
</details>

---

## excel

**模块说明**: Excel文件读写、数据处理

### excel:aci->eci

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:aci->eci (color / tmp)
  "将cad颜色索引转换为excel颜色索引\n参数:Color:cad颜色索引"
  "excel颜色索引"
  "(excel:ACI->ECI 2)"
  (if (setq tmp (vl-remove-if-not (quote (lambda (x)
            (= (cadr x)
              color)))
        *xls-color*))
    (caar tmp)
    0))
```
</details>

---

### excel:aci->truecolor

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:aci->truecolor (aci)
  "将cad颜色索引转换为真彩色值\n参数:aci:cad颜色索引"
  "真彩色值"
  "(excel:ACI->Truecolor)"
  (excel:eci->truecolor (excel:aci->eci aci)))
```
</details>

---

### excel:add-sheet

**说明**: 添加个工作表参数:XLApp:已打开的excel文件对象参数:Name:工作表名

**用法**:
```lisp
(excel:add-sheet xlapp name)
```

**参数**: 1 xlapp  : 未明确定义;2 name  : 未明确定义;

**返回值**: 成功返回t

**示例**:
```lisp
(excel:add-sheet exobj "123")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:add-sheet (xlapp name / rtn)
  "添加个工作表\n参数:XLApp:已打开的excel文件对象\n参数:Name:工作表名"
  "成功返回t"
  "(excel:add-sheet exobj \"123\")"
  (if (member name (excel:sheets xlapp))
    (setq rtn nil)
    (progn (vlax-put-property (vlax-invoke (vlax-get-property xlapp "sheets")
          "Add")
        "name"
        name)
      (setq rtn (equal (excel:get-activesheet xlapp)
          name))))
  rtn)
```
</details>

---

### excel:delete-sheet

**说明**: 说明:删除工作表参数:XLApp:已打开的excel文件对象参数:Name:工作表名

**用法**:
```lisp
(excel:delete-sheet xlapp name)
```

**参数**: 1 xlapp  : 未明确定义;2 name  : 未明确定义;

**返回值**: 成功返回t

**示例**:
```lisp
(excel:deleteSheet exobj "123")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:delete-sheet (xlapp name / old rtn sh)
  "说明:删除工作表\n参数:XLApp:已打开的excel文件对象\n参数:Name:工作表名"
  "成功返回t"
  "(excel:deleteSheet exobj \"123\")"
  (setq rtn (excel:sheets xlapp)
    old (vlax-get-property xlapp "DisplayAlerts"))
  (vlax-put-property xlapp "DisplayAlerts"
    0)
  (vlax-for sh (vlax-get-property xlapp "sheets")
    (if (= (vlax-get-property sh "Name")
        name)
      (vlax-invoke-method sh "Delete")))
  (vlax-put-property xlapp "DisplayAlerts"
    old)
  (not (equal rtn (excel:sheets xlapp))))
```
</details>

---

### excel:eci->aci

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:eci->aci (color / tmp)
  "将excel颜色索引转换为cad颜色索引\n参数:Color:excel颜色索引"
  "cad颜色索引"
  "(Excel:ECI->ACI 6)"
  (cond ((setq tmp (assoc color *xls-color*))
      (cadr tmp))
    (t 256)))
```
</details>

---

### excel:eci->truecolor

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:eci->truecolor (color / tmp)
  "将excel颜色索引转换为真彩色值\n参数:Color:excel颜色索引"
  "真彩色值"
  "(excel:ECI->Truecolor 6)"
  (cond ((setq tmp (assoc color *xls-color*))
      (caddr tmp))
    (t 16711935)))
```
</details>

---

### excel:get-activesheet

**说明**: 获取当前工作表的名字参数:XLApp:打开的excel文件对象

**用法**:
```lisp
(excel:get-activesheet xlapp)
```

**参数**: 1 xlapp  : 未明确定义;

**返回值**: 名字字符串

**示例**:
```lisp
(excel:getActiveSheet exobj)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:get-activesheet (xlapp)
  "获取当前工作表的名字\n参数:XLApp:打开的excel文件对象"
  "名字字符串"
  "(excel:getActiveSheet exobj)"
  (vlax-get-property (vlax-get-property (vlax-get-property xlapp (quote activeworkbook))
      (quote activesheet))
    (quote name)))
```
</details>

---

### excel:get-backcolor

**说明**: 获取充填色参数:xlapp:已打开的excel文件对象参数:index:区域索引，A1引用格式或者行列表

**用法**:
```lisp
(excel:get-backcolor xlapp index)
```

**参数**: 1 xlapp  : 未明确定义;2 index  : 索引值;

**返回值**: 颜色索引字符串 0-56 号

**示例**:
```lisp
(excel:get-Backcolor exobj "A1")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:get-backcolor (xlapp index)
  "获取充填色\n参数:xlapp:已打开的excel文件对象\n参数:index:区域索引，A1引用格式或者行列表"
  "颜色索引字符串 0-56 号"
  "(excel:get-Backcolor exobj \"A1\")"
  (excel:utils-getvalue (vlax-get-property (vlax-get-property (excel:get-range xlapp index)
        (quote interior))
      (quote colorindex))))
```
</details>

---

### excel:get-mergeindex

**说明**: 获取合并单元格的索引参数:xlapp:已打开的excel文件对象参数:index:区域索引，A1引用格式或者行列表

**用法**:
```lisp
(excel:get-mergeindex xlapp index)
```

**参数**: 1 xlapp  : 未明确定义;2 index  : 索引值;

**返回值**: A1格式的索引

**示例**:
```lisp
(Excel:get-MergeIndex)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:get-mergeindex (xlapp index / rtn)
  "获取合并单元格的索引\n参数:xlapp:已打开的excel文件对象\n参数:index:区域索引，A1引用格式或者行列表"
  "A1格式的索引"
  "(Excel:get-MergeIndex)"
  (if (excel:range-mergep xlapp index)
    (progn (vlax-invoke-method (excel:get-range xlapp index)
        (quote select))
      (setq rtn (excel:get-selection xlapp))))
  rtn)
```
</details>

---

### excel:get-property

**说明**: 检索 VLA 对象的特性参数:obj:vla对象参数:prop:符号或字符串，标识要检索的特性，字符串的时候可以直接调用多级特性："Rows.Count"

**用法**:
```lisp
(excel:get-property obj prop)
```

**参数**: 1 obj  : activeX 对象;2 prop  : 未明确定义;

**返回值**: 特性的值

**示例**:
```lisp
(excel:get-property range "MergeArea.Rows.Count")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:get-property (obj prop / rtn)
  "检索 VLA 对象的特性\n参数:obj:vla对象\n参数:prop:符号或字符串，标识要检索的特性，字符串的时候可以直接调用多级特性：\"Rows.Count\""
  "特性的值"
  "(excel:get-property range \"MergeArea.Rows.Count\")"
  (cond ((= (type prop)
        (quote sym))
      (setq rtn (vlax-get-property obj prop)))
    ((= (type prop)
        (quote str))
      (if (null (vl-string-search "."
            prop))
        (setq rtn (vlax-get-property obj prop))
        (foreach item (bf-str->lst prop ".")
          (if (null rtn)
            (setq rtn (vlax-get-property obj item))
            (setq rtn (vlax-get-property rtn item)))))))
  rtn)
```
</details>

---

### excel:get-range

**说明**: 说明:根据索引获取range对象参数:xlapp:已打开的excel文件对象参数:index:区域索引，A1引用格式或者行列表

**用法**:
```lisp
(excel:get-range xlapp index)
```

**参数**: 1 xlapp  : 未明确定义;2 index  : 索引值;

**返回值**: range对象

**示例**:
```lisp
(excel:get-Range exobj "A1")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:get-range (xlapp index)
  "说明:根据索引获取range对象\n参数:xlapp:已打开的excel文件对象\n参数:index:区域索引，A1引用格式或者行列表"
  "range对象"
  "(excel:get-Range exobj \"A1\")"
  (vlax-get-property (vlax-get-property (vlax-get-property xlapp (quote activeworkbook))
      (quote activesheet))
    (quote range)
    (excel:utils-index-cells->range index)))
```
</details>

---

### excel:get-rangeindex

**说明**: 获取range的索引参数:range:range对象

**用法**:
```lisp
(excel:get-rangeindex range)
```

**参数**: 1 range  : 未明确定义;

**返回值**: A1格式的索引

**示例**:
```lisp
(excel:get-RangeIndex xlrange)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:get-rangeindex (range / str col row dx dy)
  "获取range的索引\n参数:range:range对象"
  "A1格式的索引"
  "(excel:get-RangeIndex xlrange)"
  (if (equal (excel:get-property range "mergecells")
      :vlax-true)
    (setq str "MergeArea.")
    (setq str ""))
  (setq dx (excel:get-property range (strcat str "Rows.Count"))
    dy (excel:get-property range (strcat str "Columns.Count"))
    row (excel:get-property range (strcat str "Row"))
    col (excel:get-property range (strcat str "Column")))
  (excel:utils-index-cells->range (list row col (1- (+ row dx))
      (1- (+ col dy)))))
```
</details>

---

### excel:get-rangevalue

**说明**: 获取单元格或区域的值参数:XLApp:已打开的excel文件对象参数:index:位置信息，如"A1"或者'(1 1), "A1:B2"或者'(1 1 2 2)

**用法**:
```lisp
(excel:get-rangevalue xlapp index)
```

**参数**: 1 xlapp  : 未明确定义;2 index  : 索引值;

**返回值**: 值的列表

**示例**:
```lisp
(excel:get-RangeValue exobj "A1:B2")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:get-rangevalue (xlapp index)
  "获取单元格或区域的值\n参数:XLApp:已打开的excel文件对象\n参数:index:位置信息，如\"A1\"或者'(1 1), \"A1:B2\"或者'(1 1 2 2)"
  "值的列表"
  "(excel:get-RangeValue exobj \"A1:B2\")"
  (excel:utils-getvalue (vlax-get-property (excel:get-range xlapp index)
      (quote value2))))
```
</details>

---

### excel:get-selection

**说明**: 获取选择区域的索引参数:xlapp:已打开的excel文件对象

**用法**:
```lisp
(excel:get-selection xlapp)
```

**参数**: 1 xlapp  : 未明确定义;

**返回值**: A1格式的索引

**示例**:
```lisp
(excel:get-Selection exobj)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:get-selection (xlapp)
  "获取选择区域的索引\n参数:xlapp:已打开的excel文件对象"
  "A1格式的索引"
  "(excel:get-Selection exobj)"
  (excel:get-rangeindex (vlax-get-property xlapp (quote selection))))
```
</details>

---

### excel:get-usedrange

**说明**: 获取已使用的range区域参数:XLApp:已打开的excel文件对象参数:Name:工作表名

**用法**:
```lisp
(excel:get-usedrange xlapp name)
```

**参数**: 1 xlapp  : 未明确定义;2 name  : 未明确定义;

**返回值**: 成功返回range对象

**示例**:
```lisp
(excel:get-UsedRange exobj "345")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:get-usedrange (xlapp name / rtn sh)
  "获取已使用的range区域\n参数:XLApp:已打开的excel文件对象\n参数:Name:工作表名"
  "成功返回range对象"
  "(excel:get-UsedRange exobj \"345\")"
  (if (null name)
    (setq name (excel:get-activesheet xlapp)))
  (vlax-for sh (vlax-get-property xlapp "sheets")
    (if (= (vlax-get-property sh "Name")
        name)
      (setq rtn (vlax-get-property sh "UsedRange"))))
  rtn)
```
</details>

---

### excel:merge-range

**说明**: 合并单元格参数:xlapp:已打开的excel文件对象参数:index:区域索引，A1引用格式或者行列表

**用法**:
```lisp
(excel:merge-range xlapp index)
```

**参数**: 1 xlapp  : 未明确定义;2 index  : 索引值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:merge-range (xlapp index / range)
  "合并单元格\n参数:xlapp:已打开的excel文件对象\n参数:index:区域索引，A1引用格式或者行列表"
  (vlax-invoke-method (excel:get-range xlapp (excel:utils-index-cells->range index))
    (quote merge))
  (excel:get-range xlapp index))
```
</details>

---

### excel:new

**说明**: 新建Excel工作簿参数:ishide:是否可见，t为可见，nil为不可见

**用法**:
```lisp
(excel:new ishide)
```

**参数**: 1 ishide  : 未明确定义;

**返回值**: 一个表示Excel工作簿的vla对象

**示例**:
```lisp
(excel:New t)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:new (ishide / rtn)
  "新建Excel工作簿\n参数:ishide:是否可见，t为可见，nil为不可见"
  "一个表示Excel工作簿的vla对象"
  "(excel:New t)"
  (if (setq rtn (vlax-get-or-create-object "Excel.Application"))
    (progn (vlax-invoke (vlax-get-property rtn (quote workbooks))
        (quote add))
      (if ishide (vla-put-visible rtn 1)
        (vla-put-visible rtn 0))))
  rtn)
```
</details>

---

### excel:open

**说明**: 打开一个excel文件参数:Filename:文件路径参数:ishide:是否可见，t为可见，nil为不可见

**用法**:
```lisp
(excel:open filename ishide)
```

**参数**: 1 filename  : 文件名;2 ishide  : 未明确定义;

**返回值**: 一个表示打开的excel文件的vla对象

**示例**:
```lisp
(excel:open "C:\Users\mimi\Desktop\1.xlsx" t)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:open (filename ishide / activesheet excelapp rtn sheets worksheet)
  "打开一个excel文件\n参数:Filename:文件路径\n参数:ishide:是否可见，t为可见，nil为不可见"
  "一个表示打开的excel文件的vla对象"
  "(excel:open \"C:\\Users\\mimi\\Desktop\\1.xlsx\"
    t)"
  (if (and (findfile filename)
      (setq rtn (vlax-get-or-create-object "Excel.Application")))
    (progn (vlax-invoke (vlax-get-property rtn (quote workbooks))
        (quote open)
        filename)
      (if ishide (vla-put-visible rtn 1)
        (vla-put-visible rtn 0))))
  rtn)
```
</details>

---

### excel:quit

**说明**: 退出excel参数:ExlObj:打开的excel对象参数:SaveYN:是否保存，t为保存，nil为不保存

**用法**:
```lisp
(excel:quit exlobj saveyn)
```

**参数**: 1 exlobj  : 未明确定义;2 saveyn  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:quit (exlobj saveyn)
  "退出excel\n参数:ExlObj:打开的excel对象\n参数:SaveYN:是否保存，t为保存，nil为不保存"
  (if saveyn (vlax-invoke (vlax-get-property exlobj "ActiveWorkbook")
      (quote close))
    (vlax-invoke (vlax-get-property exlobj "ActiveWorkbook")
      (quote close)
      :vlax-false))
  (vlax-invoke exlobj (quote quit))
  (vlax-release-object exlobj)
  (setq exlobj nil)
  (gc))
```
</details>

---

### excel:quit-all

**说明**: 退出所有打开的excel文件参数:SaveYN:是否保存

**用法**:
```lisp
(excel:quit-all saveyn)
```

**参数**: 1 saveyn  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:quit-all (saveyn / exlobj)
  "退出所有打开的excel文件\n参数:SaveYN:是否保存"
  (while (setq exlobj (vlax-get-object "Excel.Application"))
    (excel:quit exlobj saveyn)))
```
</details>

---

### excel:range-mergep

**说明**: 判断是否是合并单元格参数:xlapp:已打开的excel文件对象参数:index:区域索引，A1引用格式或者行列表

**用法**:
```lisp
(excel:range-mergep xlapp index)
```

**参数**: 1 xlapp  : 未明确定义;2 index  : 索引值;

**返回值**: 是，返回t，否，返回nil

**示例**:
```lisp
(excel:Range-Mergep exobj "A1")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:range-mergep (xlapp index)
  "判断是否是合并单元格\n参数:xlapp:已打开的excel文件对象\n参数:index:区域索引，A1引用格式或者行列表"
  "是，返回t，否，返回nil"
  "(excel:Range-Mergep exobj \"A1\")"
  (equal (vlax-variant-value (vlax-get-property (excel:get-range xlapp (excel:utils-index-cells->range index))
        (quote mergecells)))
    :vlax-true))
```
</details>

---

### excel:rename-sheet

**说明**: 说明:重命名工作表参数:XLApp:已打开的excel文件对象参数:Old:工作表原名参数:New:工作表新名

**用法**:
```lisp
(excel:rename-sheet xlapp old new)
```

**参数**: 1 xlapp  : 未明确定义;2 old  : 未明确定义;3 new  : 未明确定义;

**返回值**: 成功返回t

**示例**:
```lisp
(excel:rename-Sheet  exobj "123" "345")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:rename-sheet (xlapp old new / rtn sh)
  "说明:重命名工作表\n参数:XLApp:已打开的excel文件对象\n参数:Old:工作表原名\n参数:New:工作表新名"
  "成功返回t"
  "(excel:rename-Sheet  exobj \"123\"
    \"345\")"
  (if (null old)
    (setq old (excel:get-activesheet xlapp)))
  (if (member new (excel:sheets xlapp))
    (setq rtn nil)
    (progn (vlax-for sh (vlax-get-property xlapp "sheets")
        (if (= (vlax-get-property sh (quote name))
            old)
          (vlax-put-property sh (quote name)
            new)))
      (setq rtn (equal new (excel:get-activesheet xlapp)))))
  rtn)
```
</details>

---

### excel:save

**说明**: 保存当前工作簿参数:xlsApp:当前工作簿对象

**用法**:
```lisp
(excel:save xlsapp)
```

**参数**: 1 xlsapp  : 未明确定义;

**返回值**: 正确保存应该返回t，错误返回nil

**示例**:
```lisp
(excel:save xlsobj)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:save (xlsapp)
  "保存当前工作簿\n参数:xlsApp:当前工作簿对象"
  "正确保存应该返回t，错误返回nil"
  "(excel:save xlsobj)"
  (equal (vlax-invoke-method (vlax-get-property xlsapp "ActiveWorkbook")
      "Save")
    :vlax-true))
```
</details>

---

### excel:saveas

**说明**: 另存为excel文件参数:XLApp:已打开的excel文件对象参数:Filename:另存为的文件路径

**用法**:
```lisp
(excel:saveas xlapp filename)
```

**参数**: 1 xlapp  : 未明确定义;2 filename  : 文件名;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:saveas (xlapp filename)
  "另存为excel文件\n参数:XLApp:已打开的excel文件对象\n参数:Filename:另存为的文件路径"
  (vlax-invoke (vlax-get-property xlapp "ActiveWorkbook")
    "SaveAs"
    filename))
```
</details>

---

### excel:set-activesheet

**说明**: 设置活动工作表参数:XLApp:已打开的excel文件对象参数:Name:工作表名

**用法**:
```lisp
(excel:set-activesheet xlapp name)
```

**参数**: 1 xlapp  : 未明确定义;2 name  : 未明确定义;

**返回值**: 成功返回t

**示例**:
```lisp
(excel:set-ActiveSheet exobj "123")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:set-activesheet (xlapp name / sh)
  "设置活动工作表\n参数:XLApp:已打开的excel文件对象\n参数:Name:工作表名"
  "成功返回t"
  "(excel:set-ActiveSheet exobj \"123\")"
  (if (member name (excel:sheets xlapp))
    (vlax-for sh (vlax-get-property xlapp "sheets")
      (if (= (vlax-get-property sh "Name")
          name)
        (vlax-invoke-method sh "Activate"))))
  (equal (excel:get-activesheet xlapp)
    name))
```
</details>

---

### excel:set-backcolor

**说明**: 设置充填色参数:xlapp:已打开的excel文件对象参数:index:区域索引，A1引用格式或者行列表参数:colorindex:颜色索引0-56号

**用法**:
```lisp
(excel:set-backcolor xlapp index colorindex)
```

**参数**: 1 xlapp  : 未明确定义;2 index  : 索引值;3 colorindex  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:set-backcolor (xlapp index colorindex)
  "设置充填色\n参数:xlapp:已打开的excel文件对象\n参数:index:区域索引，A1引用格式或者行列表\n参数:colorindex:颜色索引0-56号"
  (vlax-put-property (vlax-get-property (excel:get-range xlapp index)
      (quote interior))
    (quote colorindex)
    colorindex))
```
</details>

---

### excel:set-rangevalue

**说明**: 设置单元格或区域的值参数:XLApp:已打开的excel文件对象参数:index:位置信息，如"A1"或者'(1 1), "A1:B2"或者'(1 1 2 2)参数:value:要设置的值列表或者字符串/数字等

**用法**:
```lisp
(excel:set-rangevalue xlapp index value)
```

**参数**: 1 xlapp  : 未明确定义;2 index  : 索引值;3 value  : 值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:set-rangevalue (xlapp index value / range)
  "设置单元格或区域的值\n参数:XLApp:已打开的excel文件对象\n参数:index:位置信息，如\"A1\"或者'(1 1), \"A1:B2\"或者'(1 1 2 2)\n参数:value:要设置的值列表或者字符串/数字等"
  (setq range (excel:get-range xlapp index))
  (if (= (quote list)
      (type value))
    (progn (vlax-for it range (vlax-put-property it (quote value2)
          (car value))
        (setq value (cdr value))))
    (progn (vlax-for it range (vlax-put-property it (quote value2)
          value)))))
```
</details>

---

### excel:sheets

**说明**: 获取工作表列表参数:XLApp:已打开的excel文件对象

**用法**:
```lisp
(excel:sheets xlapp)
```

**参数**: 1 xlapp  : 未明确定义;

**返回值**: 工作表名列表

**示例**:
```lisp
(excel:sheets exobj)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:sheets (xlapp / rtn sh)
  "获取工作表列表\n参数:XLApp:已打开的excel文件对象"
  "工作表名列表"
  "(excel:sheets exobj)"
  (vlax-for sh (vlax-get-property xlapp "sheets")
    (setq rtn (cons (vlax-get-property sh "Name")
        rtn)))
  (reverse rtn))
```
</details>

---

### excel:unmerge-range

**说明**: 分解合并单元格参数:xlapp:已打开的excel文件对象参数:index:区域索引，A1引用格式或者行列表

**用法**:
```lisp
(excel:unmerge-range xlapp index)
```

**参数**: 1 xlapp  : 未明确定义;2 index  : 索引值;

**返回值**: 分解后的range对象

**示例**:
```lisp
(excel:UnmergeRange exobj "A1")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:unmerge-range (xlapp index / rtn)
  "分解合并单元格\n参数:xlapp:已打开的excel文件对象\n参数:index:区域索引，A1引用格式或者行列表"
  "分解后的range对象"
  "(excel:UnmergeRange exobj \"A1\")"
  (setq index (excel:utils-index-cells->range index))
  (if (excel:range-mergep xlapp index)
    (progn (vlax-invoke-method (excel:get-range xlapp index)
        (quote unmerge))
      (setq rtn (excel:get-range xlapp index))))
  rtn)
```
</details>

---

### excel:utils-getvalue

**说明**: 说明:工具函数，获取变体的值参数:var:变体

**用法**:
```lisp
(excel:utils-getvalue var)
```

**参数**: 1 var  : 未明确定义;

**返回值**: 值列表，其中数字全部转换为字符串

**示例**:
```lisp
(excel:Utils-GetValue obj)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:utils-getvalue (var)
  "说明:工具函数，获取变体的值\n参数:var:变体"
  "值列表，其中数字全部转换为字符串"
  "(excel:Utils-GetValue obj)"
  (cond ((= (quote list)
        (type var))
      (mapcar (quote excel:utils-getvalue)
        var))
    ((= (quote variant)
        (type var))
      (excel:utils-getvalue (vlax-variant-value (if (member (vlax-variant-type var)
              (quote (5 4 3 2)))
            (setq var (vlax-variant-change-type var vlax-vbstring))
            var))))
    ((= (quote safearray)
        (type var))
      (mapcar (quote excel:utils-getvalue)
        (vlax-safearray->list var)))
    (t var)))
```
</details>

---

### excel:utils-index-cells->range

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:utils-index-cells->range (lst / num->col)
  "说明:工具函数，将行号、列标表转换成A1格式的引用\n参数:lst:行号、列标表，列最多支持到ZZ列"
  "A1格式的引用"
  "(Excel:Utils-index-cells->range '(1 2 3 4))"
  (defun num->col (n)
    (if (< n 27)
      (chr (+ 64 n))
      (strcat (num->col (/ (1- n)
            26))
        (num->col (1+ (rem (1- n)
              26))))))
  (if (= (quote list)
      (type lst))
    (cond ((= 2 (length lst))
        (strcat (num->col (cadr lst))
          (itoa (car lst))))
      ((= 4 (length lst))
        (strcat (num->col (cadr lst))
          (itoa (car lst))
          ":"
          (num->col (last lst))
          (itoa (caddr lst))))
      (t "A1"))
    lst))
```
</details>

---

### excel:utils-index-offset

**说明**: 根据行列偏移量计算单元格索引参数:BaseCellId:基础单元格索引，可以为A1引用格式或者行列数字列表参数:rowOffset:行偏移量参数:columnOffset:列偏移量

**用法**:
```lisp
(excel:utils-index-offset basecellid rowoffset columnoffset)
```

**参数**: 1 basecellid  : 未明确定义;2 rowoffset  : 未明确定义;3 columnoffset  : 未明确定义;

**返回值**: A1格式的单元格索引

**示例**:
```lisp
(excel:Utils-index-offset "A1" 2 3)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:utils-index-offset (basecellid rowoffset columnoffset)
  "根据行列偏移量计算单元格索引\n参数:BaseCellId:基础单元格索引，可以为A1引用格式或者行列数字列表\n参数:rowOffset:行偏移量\n参数:columnOffset:列偏移量"
  "A1格式的单元格索引"
  "(excel:Utils-index-offset \"A1\"
    2 3)"
  (if (= (quote str)
      (type basecellid))
    (setq basecellid (excel:utils-index-range->cells basecellid)))
  (excel:utils-index-cells->range (mapcar (quote +)
      basecellid (list rowoffset columnoffset))))
```
</details>

---

### excel:utils-index-range->cells

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun excel:utils-index-range->cells (var / index str->list)
  "工具函数，将A1格式的引用转换成行号、列标表\n参数:var:A1格式的字符串"
  "行号、列标表"
  "(excel:Utils-index-range->cells \"DD23:EE44\")"
  (defun str->list (str)
    (list (read (vl-list->string (vl-remove-if-not (quote (lambda (x)
                (<= 48 x 57)))
            (vl-string->list str))))
      ((lambda (f)
          (f (reverse (vl-remove-if (quote (lambda (x)
                    (<= 48 x 57)))
                (vl-string->list str)))))
        (lambda (l)
          (if l (+ (* 26 (f (cdr l)))
              (- (car l)
                64))
            0)))))
  (if (setq index (vl-string-position 58 var))
    (append (str->list (substr var 1 index))
      (str->list (substr var (+ 2 index))))
    (str->list var)))
```
</details>

---

## ext

**模块说明**: 专用功能模块

### ext:anchor-hiddenfun

**说明**: 显化AutoCAD 隐藏的函数, fun 隐藏函数名；prefix 显化函数的前缀.

**用法**:
```lisp
(ext:anchor-hiddenfun fun prefix)
```

**参数**: 1 fun  : 未识别定义;2 prefix  : 未识别定义;

**示例**:
```lisp
(ext:anchor-hiddenfun 'beep 'at-)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ext:anchor-hiddenfun (fun prefix / dat file fo len fun1)
  "显化AutoCAD 隐藏的函数, fun 隐藏函数名；prefix 显化函数的前缀."
  ""
  "(ext:anchor-hiddenfun 'beep 'at-)"
  (if (= (type fun)
      (quote sym))
    (setq fun (strcase (vl-symbol-name fun)
        t)))
  (cond ((null prefix)
      (setq prefix ""))
    ((= (type prefix)
        (quote sym))
      (setq prefix (strcase (vl-symbol-name prefix)
          t)))
    ((/= (quote str)
        (type prefix))
      (setq prefix ""))
    (t (setq prefix "")))
  (setq fun1 (strcat prefix fun))
  (setq len (+ (strlen fun)
      (strlen fun1)
      28))
  (setq file (strcat @:*prefix* ".cache\\"
      FUN ".fas"))
  (SETQ DAT (APPEND (QUOTE (13 266 32))
      (VL-STRING->LIST "fas4-file ; do not change it!")
      (QUOTE (13 266 49 13 266 49 32 36 32 36))
      (QUOTE (13 266))
      (VL-STRING->LIST (ITOA LEN))
      (QUOTE (32 52 32 36 20 1 1 1 256 219))
      (VL-STRING->LIST FUN1)
      (QUOTE (256 256 214))
      (VL-STRING->LIST FUN)
      (QUOTE (256 256 1 67 256 256 2 256 266 266 131 1 256 160 134 256 256 1 22 36 59))
      (VL-STRING->LIST "@lisp")))
  (IF (SETQ FO (OPEN FILE "w"))
    (PROGN (FOREACH X DAT (WRITE-CHAR X FO))
      (CLOSE FO)
      (LOAD FILE)
      (EVAL (READ FUN)))
    (PRINC (STRCAT "manifest faile."))))
```
</details>

---

### ext:interior

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ext:interior (fun / out lst-int ifun)
  "显化内部符号或函数。"
  "subr"
  "(ext:interior 'beep)"
  (cond 
   ((listp fun)
    (foreach x fun (ext:interior x)))
   ((atom fun)
    (if (= (type fun) 'sym)(setq fun (strcase (vl-symbol-name fun) t)))
    (if (= 'subr (type (eval (read fun))))
  (eval(read fun))
      (progn
  (setq out (strcat @:*prefix* ".cache" (chr 92) "interior-"
        (filename:replace-special fun)
        ".fas"))
  (if (findfile out)
      (load out)
    (progn
      (setq lst-int
      (append  (vl-string->list "\r\n FAS4-FILE ; Do not change it!\r\n")
         '(49 13 49 32 36 7 36 13 )
         (vl-string->list (itoa (+ 26 (* 2 (strlen fun)))))
         (list 32 50 32 36 20 1 1 1 0 86)
         (vl-string->list fun)
         (list 0 0 91 )
         (vl-string->list (vl-string-subst "-" "." fun))
         (list 0 0 1 67 0 0 2 0 10 3 0 0 11 6 1 0 22 36)
         (vl-string->list "\n;@lisp https://atlisp.cn")
         ))
      (file:list-to-stream out lst-int)
      (load out))))))))
```
</details>

---

### ext:interior-fun

**说明**: 调用内部函数。

**用法**:
```lisp
(ext:interior-fun fun)
```

**参数**: 1 fun  : 函数名;

**返回值**: Subr

**示例**:
```lisp
((ext:interior-fun 'beep) 523 200)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ext:interior-fun (fun / out lst-int ifun)
  "调用内部函数。"
  "subr"
  "((ext:interior-fun 'beep) 523 200)"
  (if (= (type fun) 'sym)(setq fun (strcase (vl-symbol-name fun) t)))
  (if (= 'subr (type (eval (read fun))))
      (eval(read fun))
    (progn
      (setq out (strcat @:*prefix* ".cache" (chr 92) "interior-"
      (filename:replace-special fun)
      ".fas"))
      (if (findfile out)
    (load out)
  (progn
    (setq lst-int
    (append  (vl-string->list "\r\n FAS4-FILE ; Do not change it!\r\n")
       '(49 13 49 32 36 7 36 13 )
       (vl-string->list (itoa (+ 26 (* 2 (strlen fun)))))
       (list 32 50 32 36 20 1 1 1 0 86)
       (vl-string->list fun)
       (list 0 0 91 )
       (vl-string->list (vl-string-subst "-" "." fun))
       (list 0 0 1 67 0 0 2 0 10 3 0 0 11 6 1 0 22 36)
       (vl-string->list "\n;@lisp https://atlisp.cn")
       ))
    (file:list-to-stream out lst-int)
    (load out))))))
```
</details>

---

### ext:probe-fun

**说明**: 探测函数的参数个数(最小)及参数类型。

**用法**:
```lisp
(ext:probe-fun fun)
```

**参数**: 1 fun  : 未识别定义;

**返回值**: expr or nil

**示例**:
```lisp
(ext:probe-fun 'boole)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ext:probe-fun (fun / args iter iter-depth)
  "探测函数的参数个数(最小)及参数类型。"
  "expr or nil"
  "(ext:probe-fun 'boole)"
  (setq args (quote nil))
  (setq iter-depth 0)
  (setq str "string"
    intnum (fix 5)
    num 3.14 ent (entity:make-circle (quote (0 0 0))
      100)
    vlaobj (e2o ent)
    ss (ssadd ent)
    pt (quote (1 1 0)))
  (defun get-type (x)
    (cond ((listp x)
        (cond ((equal x (quote (1 1 0)))
            (quote point))
          (t (quote lst))))
      (t (type x))))
  (defun iter nil (setq iter-depth (1+ iter-depth))
    (if (and (vl-catch-all-error-p (setq errobj (vl-catch-all-apply fun (reverse args))))
        (setq errmsg (vl-catch-all-error-message errobj)))
      (if (< iter-depth 100)
        (cond ((wcmatch errmsg "*参数太少*")
            (setq args (cons (read (strcat "arg"
                    (itoa (length args))))
                args))
            (iter))
          ((wcmatch errmsg "参数类型错误*")
            (@:log "INFO"
              errmsg)
            (setq typeerr (string:to-list errmsg "
                "))
            (@:log "INFO"
              (vl-prin1-to-string typeerr))
            (cond ((wcmatch (cadr typeerr)
                  "stringp*")
                (setq args (subst str (read (last typeerr))
                    args)))
              ((wcmatch (cadr typeerr)
                  "listp*")
                (setq args (subst (quote (l i s t))
                    (read (last typeerr))
                    args)))
              ((wcmatch (cadr typeerr)
                  "numberp*")
                (setq args (subst num (read (last typeerr))
                    args)))
              ((wcmatch (cadr typeerr)
                  "fixnump*")
                (setq args (subst intnum (read (last typeerr))
                    args)))
              ((wcmatch (cadr typeerr)
                  "lselsetp*")
                (setq args (subst ss (read (last typeerr))
                    args)))
              ((wcmatch (cadr typeerr)
                  "lentity*")
                (setq args (subst ent (read (last typeerr))
                    args)))
              ((wcmatch (cadr typeerr)
                  "VLA-OBJECT*")
                (setq args (subst vlaobj (read (last typeerr))
                    args)))
              ((wcmatch (cadr typeerr)
                  "二维/三维点*")
                (setq args (subst pt (read (last typeerr))
                    args))))
            (iter))
          ((wcmatch errmsg "no function definition*")
            (setq args (subst (quote princ)
                (read (caddr typeerr))
                args))
            (iter))
          (t (princ errmsg)
            (princ "
               near completed \n")
            (cons fun (mapcar (quote get-type)
                (reverse args))))))
      (progn (princ "probe completed!\n")
        (cons fun (mapcar (quote get-type)
            (reverse args))))))
  (iter))
```
</details>

---

## file

**模块说明**: 文件操作、读写处理

### file:list-to-stream

**用法**:
```lisp
(file:list-to-stream out_file intlist)
```

**参数**: 1 out_file  : 未明确定义;  2 intlist  : 整数;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun file:list-to-stream (out_file intlist / adodb)
    (setq intlist (vlax-make-variant (vlax-safearray-fill (vlax-make-safearray 17 (cons 0 (1- (length intlist))))
                intlist)
            8209))
    (setq adodb (vlax-get-or-create-object "adodb.stream"))
    (vlax-put-property adodb (quote type)
        1)
    (vlax-invoke adodb (quote open))
    (vlax-put adodb (quote position)
        0)
    (vlax-invoke-method adodb (quote write)
        intlist)
    (vlax-invoke adodb (quote savetofile)
        out_file 2)
    (and adodb (vlax-invoke adodb (quote close)))
    (and adodb (vlax-release-object adodb))
    (princ))
```
</details>

---

### file:merge

**说明**: 合并多个文件内容到 dist 文件中。

**用法**:
```lisp
(file:merge dist lst-files)
```

**参数**: 1 dist  : 未识别定义;2 lst-files  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun file:merge (dist lst-files / fp-out fp-in ln)
  "合并多个文件内容到 dist 文件中。"
  (if (and (getvar "lispsys")
      (> (getvar "lispssy")
        0))
    (setq fp-out (open dist "w"
        "utf8"))
    (setq fp-out (open dist "w")))
  (foreach file% lst-files (if (findfile file%)
      (progn (setq fp-in (open file% "r"))
        (while (setq ln (read-line fp-in))
          (write-line ln fp-out))
        (close fp-in))))
  (close fp-out))
```
</details>

---

### file:parse-ini

**说明**: 解析 ini 文件。

**用法**:
```lisp
(file:parse-ini filename)
```

**参数**: 1 filename  : 文件名;

**返回值**: list

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun file:parse-ini (filename / fp result *error*)
  "解析 ini 文件。"
  "list"
  (defun *error* (msg)
    (if (= (quote file)
        (type fp))
      (close fp))
    (@:*error* msg))
  (setq fp (open filename "r"))
  (setq result (quote nil))
  (setq sub (quote nil))
  (while (setq str-line (read-line fp))
    (setq str-line (car (string:parse-by-lst (vl-string-trim "
            "
            str-line)
          (quote (";"
              "#")))))
    (cond ((= 91 (ascii str-line))
        (if sub (setq result (cons (reverse sub)
              result)))
        (setq sub (cons str-line nil)))
      ((setq a&v (string:to-list str-line "="))
        (setq sub (cons (cons (car a&v)
              (cadr a&v))
            sub)))
      (t (prompt (strcat "parse error "
            str-line)))))
  (if sub (setq result (cons (reverse sub)
        result)))
  (close fp)
  (reverse result))
```
</details>

---

### file:read-stream

**说明**: 读入指定编码的文件内容

**用法**:
```lisp
(file:read-stream filename encoding)
```

**参数**: 1 filename  : 文件名;2 encoding  : 单个图元;

**返回值**: String

**示例**:
```lisp
(file:read-stream "d:\hzfile.txt" "utf-8")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun file:read-stream (filename encoding / str stream *error*)
  "读入指定编码的文件内容"
  "String"
  "(file:read-stream \"d:\\hzfile.txt\"
    \"utf-8\")"
  (defun *error* (msg)
    (if (= (quote file)
        (type stream))
      (close stream))
    (princ msg)
    (princ))
  (if (>= (atof (getvar "acadver"))
      24)
    (progn (setq stream (open filename "r"))
      (setq str "")
      (while (setq str-line (read-line stream))
        (setq str (strcat str "\n"
            str-line)))
      (close stream))
    (progn (setq stream (vlax-create-object "ADODB.Stream"))
      (vlax-put-property stream (quote type)
        2)
      (vlax-put-property stream (quote charset)
        encoding)
      (vlax-invoke stream (quote open))
      (vlax-invoke-method stream (quote loadfromfile)
        filename)
      (setq str (vlax-invoke-method stream (quote readtext)
          -1))
      (vlax-invoke-method stream (quote close))
      (vlax-release-object stream)))
  str)
```
</details>

---

### file:remove-lines

**说明**: 删除文件 file 中 某几行的内容，或内容为 lines 的行

**用法**:
```lisp
(file:remove-lines file lines)
```

**参数**: 1 file  : 文件名;2 lines  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun file:remove-lines (file lines / tmpfp)
  "删除文件 file 中 某几行的内容，或内容为 lines 的行"
  (if (atom lines)
    (setq lines (list lines)))
  (if (findfile file)
    (progn (setq fp (open file "r"))
      (setq tmpfp (open (setq tmpfile (vl-filename-mktemp))
          "w"))
      (setq n 0)
      (while (setq content (read-line fp))
        (setq n (1+ n))
        (if (not (or (member n lines)
              (member content lines)))
          (write-line content tmpfp)))
      (close fp)
      (close tmpfp)
      (vl-file-delete file)
      (vl-file-rename tmpfile file))
    (princ "Not found file"))
  (princ))
```
</details>

---

### file:subst-all

**说明**: 替换文件中的字符串。

**用法**:
```lisp
(file:subst-all newstr oldstr lspfile new-suffix)
```

**参数**: 1 newstr  : 未明确定义;  2 oldstr  : 未明确定义;  3 lspfile  : 未明确定义;  4 new-suffix  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun file:subst-all (newstr oldstr lspfile new-suffix / newfile vf zf text)
    "替换文件中的字符串。"
    (setq newfile (strcat (vl-filename-directory lspfile)
            "\\"
            (vl-filename-base lspfile)
            new-suffix (vl-filename-extension lspfile)))
    (if (findfile newfile)
        (vl-file-delete (findfile newfile)))
    (setq vf (open lspfile "r"))
    (setq zf (open newfile "w"))
    (while (setq text (read-line vf))
        (write-line (string:subst-all newstr oldstr text)
            zf))
    (close vf)
    (close zf))
```
</details>

---

## filename

**模块说明**: 专用功能模块

### filename:replace-special

**说明**: 替换文件名中的特殊字符

**用法**:
```lisp
(filename:replace-special str)
```

**参数**: 1 str  : 字符串;

**返回值**: String

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun filename:replace-special (str)
  "替换文件名中的特殊字符,本函数只适用于单个文件，不适用于带目录的文件名字符串。"
  "String"
  "(filename:replace-special \":s->l.lsp\")"
  (foreach x '(("+" . "*")
         ("／" . "/")
         ("：" . ":")
         ("；" . ";")
         ("｜" . "|")
         ("、" . "\\")
         ("-to-" . "->")
         ("gt-" . ">")
         ("-" . "\"")
         )
     (setq str (@:string-subst
          (car x)(cdr x)
          str))))
```
</details>

---

### filename:special-symbol

**说明**: 替换文件名中的特殊字符

**用法**:
```lisp
(filename:special-symbol str)
```

**参数**: 1 str  : 字符串;

**返回值**: String

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun filename:special-symbol (str)
  "替换文件名中的特殊字符,本函数只适用于单个文件，不适用于带目录的文件名字符串。"
  "String"
  "(filename:replace-special \":s->l.lsp\")"
  (foreach x '(("／" . "/")
    ("：" . ":")
    ("；" . ";")
    ("｜" . "|")
    ("、" . "\\")
    ("-to-" . "->")
    ("gt-" . ">")
    ("-" . "\"")
    )
     (setq str (@:string-subst
          (car x)(cdr x)
          str))))
```
</details>

---

## fun

**模块说明**: 专用功能模块

### fun:test

**说明**: 函数测试，fun:函数名，usecases:测试用例，测试用例为参数和返回值组成的点对表。

**用法**:
```lisp
(fun:test fun usecases)
```

**参数**: 1 fun  : 未识别定义;2 usecases  : 未识别定义;

**返回值**: lst,对应测试用例的测试结果，t为通过，nil为失败

**示例**:
```lisp
(fun:test point:mid '((((0 0)(2 2)) . (1 1))(((3 3)(5 5)) . (4 4))))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun fun:test (fun usecases)
  "函数测试，fun:函数名，usecases:测试用例，测试用例为参数和返回值组成的点对表。"
  "lst,对应测试用例的测试结果，t为通过，nil为失败"
  "(fun:test point:mid '((((0 0)(2 2))
        . (1 1))(((3 3)(5 5))
        . (4 4))))"
  (mapcar (quote (lambda (x)
        (list:equal (apply (function fun)
            (car x))
          (cdr x)
          1.0e-06)))
    usecases))
```
</details>

---

## g

**模块说明**: 专用功能模块

### g:dot-product

**说明**: 求两个向量的点积、内积、标量积、数量积

**用法**:
```lisp
(g:dot-product v1 v2)
```

**参数**: 1 v1  : 未识别定义;2 v2  : 未识别定义;

**返回值**: number

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun g:dot-product (v1 v2)
  "求两个向量的点积、内积、标量积、数量积"
  "number"
  (apply (quote +)
    (mapcar (quote *)
      v1 v2)))
```
</details>

---

### g:foot-point

**说明**: 求点到直线的垂足的坐标，line 为两点坐标组成的列表，pt 为一点坐标

**用法**:
```lisp
(g:foot-point line pt)
```

**参数**: 1 line  : 未识别定义;2 pt  : 单个2D/3D坐标点;

**返回值**: 坐标

**示例**:
```lisp
(g:foot-point (list (getpoint)(getpoint)) (getpoint))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun g:foot-point (line pt / dis dot-product foot projection v-p v-w)
  "求点到直线的垂足的坐标，line 为两点坐标组成的列表，pt 为一点坐标"
  "坐标"
  "(g:foot-point (list (getpoint)(getpoint))
    (getpoint))"
  (setq v-w (mapcar (quote -)
      (cadr line)
      (car line)))
  (setq v-p (mapcar (quote -)
      pt (car line)))
  (setq dot-product (g:dot-product v-w v-p))
  (setq dis (apply (quote distance)
      line))
  (setq projection (/ dot-product (expt dis 2)))
  (setq foot-point (list (+ (caar line)
        (* (car v-w)
          projection))
      (+ (cadar line)
        (* (cadr v-w)
          projection))
      (+ (caddr (car line))
        (* (caddr v-w)
          projection)))))
```
</details>

---

## geometry

**模块说明**: 几何计算、坐标变换

### geometry:angle

**说明**: 直线(线段)与坐标轴xyz的夹角列表

**用法**:
```lisp
(geometry:angle segment)
```

**参数**: 1 segment  : 线段，两点组成的表，可以表示有向图或无向图的一个边。;

**返回值**: 两点直线与x y z 轴的夹角(弧度)

**示例**:
```lisp
(geometry:angel '((0 0 0)(1 1 1)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun geometry:angle (segment / pt1 pt2 dist-o)
  "直线(线段)与坐标轴xyz的夹角列表"
  "两点直线与x y z 轴的夹角(弧度)"
  "(geometry:angel '((0 0 0)(1 1 1)))"
  (setq pt1 (car segment)
    pt2 (cadr segment))
  (setq dist-o (sqrt (+ (* (- (car pt2)
            (car pt1))
          (- (car pt2)
            (car pt1)))
        (* (- (cadr pt2)
            (cadr pt1))
          (- (cadr pt2)
            (cadr pt1)))
        (* (- (caddr pt2)
            (caddr pt1))
          (- (caddr pt2)
            (caddr pt1))))))
  (list (/ (- (car pt2)
        (car pt1))
      dist-o)
    (/ (- (cadr pt2)
        (cadr pt1))
      dist-o)
    (/ (- (caddr pt2)
        (caddr pt1))
      dist-o)))
```
</details>

---

### geometry:box-intersectp

**说明**: 测试两个盒子是否交叉

**用法**:
```lisp
(geometry:box-intersectp box1 box2)
```

**参数**: 1 box1  : 未明确定义;2 box2  : 未明确定义;

**返回值**: T 交叉，nil 不交叉

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun geometry:box-intersectp (box1 box2)
  "测试两个盒子是否交叉"
  "T 交叉，nil 不交叉"
  (not (or (< (caadr box1)
        (caar box2))
      (< (cadadr box1)
        (cadar box2))
      (> (caar box1)
        (caadr box2))
      (> (cadar box1)
        (cadadr box2)))))
```
</details>

---

### geometry:convexhull-by-graham-scan

**说明**: graham-scan算法计算点集凸包参数: pts:点表

**用法**:
```lisp
(geometry:convexhull-by-graham-scan pts)
```

**参数**: 1 pts  : 多个坐标点列表;

**返回值**: 凸包点表

**示例**:
```lisp
(geometry:convexhull-by-graham-scan '(pt1 pt2 pt3 ...))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun geometry:convexhull-by-graham-scan (pts / d i p0)
  "graham-scan算法计算点集凸包\n参数: pts:点表"
  "凸包点表"
  "(geometry:convexhull-by-graham-scan '(pt1 pt2 pt3 ...))"
  (setq pts (vl-sort pts (quote (lambda (p1 p2)
          (cond ((< (cadr p1)
                (cadr p2)))
            ((equal (cadr p1)
                (cadr p2)
                1.0e-08)
              (< (car p1)
                (car p2))))))))
  (setq p0 (car pts))
  (setq pts (vl-sort (cdr pts)
      (function (lambda (p1 p2 / m n)
          (cond ((< (setq m (angle p1 p0))
                (setq n (angle p2 p0))))
            ((equal m n 1.0e-08)
              (< (distance p1 p0)
                (distance p2 p0))))))))
  (setq pt-hull (list (cadr pts)
      (car pts)
      p0))
  (foreach curpt (cddr pts)
    (setq pt-hull (cons curpt pt-hull))
    (while (and (caddr pt-hull)
        (> 0 (geometry:turn-right-p (caddr pt-hull)
            (cadr pt-hull)
            curpt)))
      (setq pt-hull (cons curpt (cddr pt-hull)))))
  pt-hull)
```
</details>

---

### geometry:convexhull-by-jarvis

**说明**: 最小凸包算法: jarvis 步进法，package wrapping or gift wrapping

**用法**:
```lisp
(geometry:convexhull-by-jarvis pts)
```

**参数**: 1 pts  : 多个坐标点列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun geometry:convexhull-by-jarvis (pts / pfirst p0 p1 pmax1 pmax2 pp)
  "最小凸包算法: jarvis 步进法，package wrapping or gift wrapping"
  (cond ((= (length pts)
        0)
      nil)
    ((or nil (= (length pts)
          1)
        (= (length pts)
          2))
      (progn (alert "你输入的点为两点或一点!")
        pts))
    (t (progn (defun det2 (p1 p2)
          (- (* (car p1)
              (cadr p2))
            (* (car p2)
              (cadr p1))))
        (defun det (p1 p2 p3)
          (+ (det2 p1 p2)
            (det2 p2 p3)
            (det2 p3 p1)))
        (defun sign (x)
          (cond ((> x 0)
              -1.0)
            ((< x 0)
              1.0)
            (t 0)))
        (defun ang (p1 p2 p3 / x)
          (setq x (abs (- (angle p1 p3)
                (angle p1 p2))))
          (if (equal p3 p1 1.0e-08)
            (- pi)
            (if (< (abs (sin x))
                1.0e-08)
              (if (equal (- (distance p2 p3)
                    (+ (distance p1 p2)
                      (distance p1 p3)))
                  0 1.0e-08)
                pi 0)
              (if (> x pi)
                (* (- (* 2 pi)
                    x)
                  (sign (det p2 p1 p3)))
                (* x (sign (det p2 p1 p3)))))))
        (defun maxium (pts)
          (car (vl-sort pts (quote (lambda (e1 e2)
                  (if (equal (car e1)
                      (car e2)
                      1.0e-08)
                    (> (cadr e1)
                      (cadr e2))
                    (> (car e1)
                      (car e2))))))))
        (setq p0 (maxium pts))
        (setq p1 p0 pfirst p0 p0 (list (car p0)
            (+ 1.0 (cadr p0))
            (caddr p0)))
        (setq pmax1 p1)
        (setq p1 (mapcar (quote (lambda (x)
                (list (ang p1 p0 x)
                  (distance p1 x)
                  x)))
            pts))
        (setq pmax2 (caddr (maxium p1)))
        (setq pp (cons pmax2 (list pmax1)))
        (while (not (equal pfirst pmax2 1.0e-08))
          (setq p1 (mapcar (quote (lambda (x)
                  (list (ang pmax2 pmax1 x)
                    (distance pmax2 x)
                    x)))
              (mapcar (quote caddr)
                p1)))
          (setq pmax1 pmax2)
          (setq pmax2 (caddr (maxium p1)))
          (setq pp (cons pmax2 pp)))
        (reverse (cdr pp))))))
```
</details>

---

### geometry:dist-pt-line

**说明**: 求点到线段的距离

**用法**:
```lisp
(geometry:dist-pt-line pt segment)
```

**参数**: 1 pt  : 单个2D/3D坐标点;2 segment  : 线段，两点组成的表，可以表示有向图或无向图的一个边。;

**返回值**: number

**示例**:
```lisp
(geometry:dist-pt-line '(0 0 0) '((1 0 0)(0 1 0)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun geometry:dist-pt-line (pt segment / an)
  "求点到线段的距离"
  "number"
  "(geometry:dist-pt-line '(0 0 0)
    '((1 0 0)(0 1 0)))"
  (setq an (angle (car segment)
      (cadr segment)))
  (if (setq inter1 (inters (car segment)
        (cadr segment)
        pt (polar pt (+ an (* 0.5 pi))
          1000)
        nil))
    (if (geometry:on-segment inter1 segment)
      (distance pt inter1)
      (min (distance pt (car segment))
        (distance pt (cadr segment))))))
```
</details>

---

### geometry:merge-box

**说明**: 合并两个包围盒，不管两个盒子是否有重叠。

**用法**:
```lisp
(geometry:merge-box box1 box2)
```

**参数**: 1 box1  : 未明确定义;2 box2  : 未明确定义;

**返回值**: 总包围盒

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun geometry:merge-box (box1 box2 / ax1 ax2 ay1 ay2 bx1 bx2 by1 by2)
  "合并两个包围盒，不管两个盒子是否有重叠。"
  "总包围盒"
  (list (list (min (caar box1)
        (caar box2))
      (min (cadar box1)
        (cadar box2)))
    (list (max (caadr box1)
        (caadr box2))
      (max (cadadr box1)
        (cadadr box2)))))
```
</details>

---

### geometry:on-segment

**说明**: 判断一个与线段共线的点是否在线段上。

**用法**:
```lisp
(geometry:on-segment pt segment)
```

**参数**: 1 pt  : 单个2D/3D坐标点;2 segment  : 线段，两点组成的表，可以表示有向图或无向图的一个边。;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun geometry:on-segment (pt segment)
  "判断一个与线段共线的点是否在线段上。"
  (and (>= (car pt)
      (apply (quote min)
        (mapcar (quote car)
          segment)))
    (<= (car pt)
      (apply (quote max)
        (mapcar (quote car)
          segment)))
    (>= (cadr pt)
      (apply (quote min)
        (mapcar (quote cadr)
          segment)))
    (<= (cadr pt)
      (apply (quote max)
        (mapcar (quote cadr)
          segment)))))
```
</details>

---

### geometry:point-3d->2d

**用法**:
```lisp
(geometry:point-3d->2d pt)
```

**参数**: 1 pt  : 单个2D/3D坐标点;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun geometry:point-3d->2d (pt)
  (cons (car pt)
    (cadr pt)))
```
</details>

---

### geometry:segment-by-line

**用法**:
```lisp
(geometry:segment-by-line line)
```

**参数**: 1 line  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun geometry:segment-by-line (line)
  (list (entity:getdxf line 10)
    (entity:getdxf line 11)))
```
</details>

---

### geometry:segment-mid

**说明**: 求线段的中点坐标

**用法**:
```lisp
(geometry:segment-mid segment)
```

**参数**: 1 segment  : 线段，两点组成的表，可以表示有向图或无向图的一个边。;

**返回值**: 三维坐标值

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun geometry:segment-mid (segment)
  "求线段的中点坐标"
  "三维坐标值"
  (list (* 0.5 (+ (car (car segment))
        (car (cadr segment))))
    (* 0.5 (+ (cadr (car segment))
        (cadr (cadr segment))))
    (if (caddr (car segment))
      (* 0.5 (+ (caddr (car segment))
          (caddr (cadr segment))))
      0)))
```
</details>

---

### geometry:turn-left-p

**说明**: 判断三点的转角方向。

**用法**:
```lisp
(geometry:turn-left-p pt1 pt2 pt3)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;2 pt2  : 单个2D/3D坐标点;3 pt3  : 单个2D/3D坐标点;

**返回值**: 顺时针方向的夹角为正值，反之为负, 0为直线。

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun geometry:turn-left-p (pt1 pt2 pt3 / det2 det x)
  "判断三点的转角方向。"
  "顺时针方向的夹角为正值，反之为负, 0为直线。"
  (defun det2 (p1 p2)
    "矢量叉积"
    (- (* (car p1)
        (cadr p2))
      (* (car p2)
        (cadr p1))))
  (defun det (p1 p2 p3)
    "定义三点的行列式,即三点之倍面积"
    (+ (det2 p1 p2)
      (det2 p2 p3)
      (det2 p3 p1)))
  (setq x (abs (- (angle pt1 pt3)
        (angle pt1 pt2))))
  (if (equal pt3 pt1 1.0e-08)
    (- pi)
    (if (< (abs (sin x))
        1.0e-08)
      (if (equal (- (distance pt2 pt3)
            (+ (distance pt1 pt2)
              (distance pt1 pt3)))
          0 1.0e-08)
        pi 0)
      (if (> x pi)
        (* (- (* 2 pi)
            x)
          (m:sign (det pt2 pt1 pt3)))
        (* x (m:sign (det pt2 pt1 pt3)))))))
```
</details>

---

### geometry:turn-right-p

**说明**: 判断三点的转角方向。

**用法**:
```lisp
(geometry:turn-right-p pt1 pt2 pt3)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;2 pt2  : 单个2D/3D坐标点;3 pt3  : 单个2D/3D坐标点;

**返回值**: 顺时针方向的夹角为正值，反之为负, 0为直线。

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun geometry:turn-right-p (pt1 pt2 pt3 / det2 det x)
  "判断三点的转角方向。"
  "顺时针方向的夹角为正值，反之为负, 0为直线。"
  (defun det2 (p1 p2)
    "矢量叉积"
    (- (* (car p1)
        (cadr p2))
      (* (car p2)
        (cadr p1))))
  (defun det (p1 p2 p3)
    "定义三点的行列式,即三点之倍面积"
    (+ (det2 p1 p2)
      (det2 p2 p3)
      (det2 p3 p1)))
  (setq x (abs (- (angle pt1 pt3)
        (angle pt1 pt2))))
  (if (equal pt3 pt1 1.0e-08)
    (- pi)
    (if (< (abs (sin x))
        1.0e-08)
      (if (equal (- (distance pt2 pt3)
            (+ (distance pt1 pt2)
              (distance pt1 pt3)))
          0 1.0e-08)
        pi 0)
      (if (> x pi)
        (* (- (* 2 pi)
            x)
          (m:sign (det pt2 pt1 pt3)))
        (* x (m:sign (det pt2 pt1 pt3)))))))
```
</details>

---

### geometry:ucs

**用法**:
```lisp
(geometry:ucs base angle1)
```

**参数**: 1 base  : 未明确定义;2 angle1  : 角度值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun geometry:ucs (base angle1)
  (command ".ucs"
    base (polar base angle1 10000)
    (polar base (+ (* 0.25 pi)
        angle1)
      25000)))
```
</details>

---

### geometry:ucs-angle

**用法**:
```lisp
(geometry:ucs-angle )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun geometry:ucs-angle nil (if (equal 0 (car (getvar "ucsxdir"))
      1.0e-08)
    (if (equal 1 (cadr (getvar "ucsxdir"))
        1.0e-08)
      (* 0.5 pi)
      (* 1.5 pi))
    (if (>= (cadr (getvar "ucsxdir"))
        0)
      (atan (/ (cadr (getvar "ucsxdir"))
          (car (getvar "ucsxdir"))))
      (+ pi (atan (/ (cadr (getvar "ucsxdir"))
            (car (getvar "ucsxdir"))))))))
```
</details>

---

### geometry:wcs2ucs

**用法**:
```lisp
(geometry:wcs2ucs pt)
```

**参数**: 1 pt  : 单个2D/3D坐标点;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun geometry:wcs2ucs (pt)
  (m:coord-chg pt (getvar "ucsorg")
    (getvar "ucsxdir")))
```
</details>

---

## group

**模块说明**: 专用功能模块

### group:get-by-name

**说明**: 获取编组名为 name 的编组对象。

**用法**:
```lisp
(group:get-by-name name)
```

**参数**: 1 name  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun group:get-by-name (name)
    "获取编组名为 name 的编组对象。"
    (vla-item *grps* name))
```
</details>

---

### group:groups-to-objlist

**说明**: 将编组集转为编组对象列表.

**用法**:
```lisp
(group:groups-to-objlist )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun group:groups-to-objlist (/ i obj-g)
    "将编组集转为编组对象列表."
    (setq i 0)
    (setq obj-g (quote nil))
    (while (< i (vla-get-count *grps*))
        (setq obj-g (cons (vla-item *grps* i)
                obj-g))
        (setq i (1+ i)))
    obj-g)
```
</details>

---

### group:list

**说明**: 列出图中的编组名

**用法**:
```lisp
(group:list )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun group:list (/ i names)
    "列出图中的编组名"
    (setq i 0)
    (setq names (quote nil))
    (while (< i (vla-get-count *grps*))
        (setq names (cons (vla-get-name (vla-item *grps* i))
                names))
        (setq i (1+ i)))
    names)
```
</details>

---

### group:make

**说明**: 实体集编组lst 图元列表，name 编组名,(匿名组首字为*).

**用法**:
```lisp
(group:make lst name)
```

**参数**: 1 lst  : 列表;2 name  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun group:make (lst name / groupobj)
  "实体集编组\nlst 图元列表，name 编组名,(匿名组首字为*)."
  (setq groupobj (vla-add (vla-get-groups (vla-get-activedocument (vlax-get-acad-object)))
      name))
  (setq lst (mapcar (quote (lambda (x)
          (cond ((p:enamep x)
              (e2o x))
            (t x))))
      lst))
  (vla-appenditems groupobj (vla:list->array lst 9)))
```
</details>

---

### group:to-entlist

**说明**: 编组转图元列表, obj-g 为编组对象。

**用法**:
```lisp
(group:to-entlist obj-g)
```

**参数**: 1 obj-g  : activeX 对象;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun group:to-entlist (obj-g / i objlst)
    "编组转图元列表, obj-g 为编组对象。"
    (mapcar (quote vlax-vla-object->ename)
        (group:to-objlist obj-g)))
```
</details>

---

### group:to-objlist

**说明**: 编组转图元对象列表, obj-g 为编组对象。

**用法**:
```lisp
(group:to-objlist obj-g)
```

**参数**: 1 obj-g  : activeX 对象;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun group:to-objlist (obj-g / i objlst)
    "编组转图元对象列表, obj-g 为编组对象。"
    (setq i 0)
    (setq objlst (quote nil))
    (while (< i (vla-get-count obj-g))
        (setq objlst (cons (vla-item obj-g i)
                objlst))
        (setq i (1+ i)))
    objlst)
```
</details>

---

## hdinfo

**模块说明**: 专用功能模块

### hdinfo:get-cpuid

**说明**: 获取CPU ID,不一定有用。

**用法**:
```lisp
(hdinfo:get-cpuid )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun hdinfo:get-cpuid (/ vlist vobj lcom lexecquery item)
  "获取CPU ID,不一定有用。"
  (vl-load-com)
  (setq vlist (quote nil))
  (if (setq vobj (vlax-create-object "wbemscripting.swbemlocator"))
    (progn (setq lcom (vlax-invoke vobj (quote connectserver)
          "."
          "\\root\\cimv2"
          ""
          ""
          ""
          ""
          128 nil))
      (setq lexecquery (vlax-invoke lcom (quote execquery)
          "Select * from Win32_Processor"))
      (vlax-for item lexecquery (setq vlist (vlax-get item (quote processorid))))
      (vlax-release-object lexecquery)
      (vlax-release-object lcom)
      (vlax-release-object vobj)))
  vlist)
```
</details>

---

### hdinfo:get-hd-serial

**说明**: 获取硬盘序列号,不一定有用。

**用法**:
```lisp
(hdinfo:get-hd-serial )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun hdinfo:get-hd-serial (/ lccon lox objw ret serx sn)
  "获取硬盘序列号,不一定有用。"
  (setq serx (quote nil))
  (if (setq objw (vlax-create-object "wbemscripting.swbemlocator"))
    (progn (setq lccon (vlax-invoke objw (quote connectserver)
          "."
          "\\root\\cimv2"
          ""
          ""
          ""
          ""
          128 nil))
      (setq lox (vlax-invoke lccon (quote execquery)
          "select serialnumber,tag from win32_physicalmedia"))
      (vlax-for item lox (setq serx (cons (list (vlax-get item (quote tag))
              (vlax-get item (quote serialnumber)))
            serx)))
      (vlax-release-object lox)
      (vlax-release-object lccon)
      (vlax-release-object objw)))
  serx)
```
</details>

---

### hdinfo:get-mac

**说明**: 获取mac地址，不一定有用。

**用法**:
```lisp
(hdinfo:get-mac )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun hdinfo:get-mac (/ i mac s str svr wmi)
  "获取mac地址，不一定有用。"
  (vl-load-com)
  (setq wmi (vlax-create-object "WbemScripting.SWbemLocator"))
  (setq svr (vlax-invoke wmi (quote connectserver)))
  (setq str "Select * from Win32_NetworkAdapterConfiguration Where IPEnabled=TRUE")
  (setq mac (vlax-invoke svr (quote execquery)
      str))
  (vlax-for i mac (setq s (cons (vlax-get i (quote macaddress))
        s)))
  (vlax-release-object mac)
  (vlax-release-object svr)
  (vlax-release-object wmi)
  (car s))
```
</details>

---

## ini

**模块说明**: 专用功能模块

### ini:get

**说明**: 取 ini 的某项的值。lst-ini ini文件的解析结果表, node 节 ，attr 属性项

**用法**:
```lisp
(ini:get lst-ini node attr)
```

**参数**: 1 lst-ini  : 列表;2 node  : 未识别定义;3 attr  : 未识别定义;

**返回值**: String

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ini:get (lst-ini node attr)
  "取 ini 的某项的值。lst-ini ini文件的解析结果表, node 节 ，attr 属性项"
  "String"
  (setq node (strcat "["
      (vl-string-trim "[] "
        node)
      "]"))
  (cdr (assoc attr (cdr (assoc node lst-ini)))))
```
</details>

---

### ini:parse

**说明**: 解析 ini 文件。

**用法**:
```lisp
(ini:parse filename)
```

**参数**: 1 filename  : 文件名;

**返回值**: list

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ini:parse (filename / fp result *error*)
  "解析 ini 文件。"
  "list"
  (defun *error* (msg)
    (if (= (quote file)
        (type fp))
      (close fp))
    (@:*error* msg))
  (setq fp (open filename "r"))
  (setq result (quote nil))
  (setq sub (quote nil))
  (while (setq str-line (read-line fp))
    (setq str-line (car (string:parse-by-lst (vl-string-trim "
            "
            str-line)
          (quote (";"
              "#")))))
    (cond ((= 91 (ascii str-line))
        (if sub (setq result (cons (reverse sub)
              result)))
        (setq sub (cons (strcat "["
              (vl-string-trim "[] "
                str-line)
              "]")
            nil)))
      ((setq a&v (string:to-list str-line "="))
        (setq sub (cons (cons (car a&v)
              (cadr a&v))
            sub)))
      (t (prompt (strcat "parse error "
            str-line)))))
  (if sub (setq result (cons (reverse sub)
        result)))
  (close fp)
  (reverse result))
```
</details>

---

### ini:read

**说明**: 解析 ini 文件。

**用法**:
```lisp
(ini:read filename)
```

**参数**: 1 filename  : 文件名;

**返回值**: list

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ini:read (filename / fp result *error*)
  "解析 ini 文件。"
  "list"
  (defun *error* (msg)
    (if (= (quote file)
        (type fp))
      (close fp))
    (@:*error* msg))
  (setq fp (open filename "r"))
  (setq result (quote nil))
  (setq sub (quote nil))
  (while (setq str-line (read-line fp))
    (setq str-line (car (string:parse-by-lst (vl-string-trim "
            "
            str-line)
          (quote (";"
              "#")))))
    (cond ((= 91 (ascii str-line))
        (if sub (setq result (cons (reverse sub)
              result)))
        (setq sub (cons (strcat "["
              (vl-string-trim "[] "
                str-line)
              "]")
            nil)))
      ((setq a&v (string:to-list str-line "="))
        (setq sub (cons (cons (car a&v)
              (cadr a&v))
            sub)))
      (t (prompt (strcat "parse error "
            str-line)))))
  (if sub (setq result (cons (reverse sub)
        result)))
  (close fp)
  (reverse result))
```
</details>

---

### ini:save

**说明**: 保存 lst-ini 表 到 ini 文件。

**用法**:
```lisp
(ini:save lst-ini filename)
```

**参数**: 1 lst-ini  : 列表;2 filename  : 文件名;

**返回值**: T or nil

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ini:save (lst-ini filename / *error* fp)
  "保存 lst-ini 表 到 ini 文件。"
  "T or nil"
  (defun *error* (msg)
    (if (= (quote file)
        (type fp))
      (close fp))
    (@:*error* msg))
  (setq fp (open filename "w"))
  (if (= (quote file)
      (type fp))
    (progn (foreach sub lst-ini (foreach item sub (cond ((and (p:stringp item)
                (/= ""
                  item))
              (write-line (strcat "["
                  (vl-string-trim "[] "
                    item)
                  "]")
                fp))
            ((and (listp item)
                (p:stringp (car item))
                (/= ""
                  (car item)))
              (write-line (strcat (vl-string-trim "
                    "
                    (car item))
                  "="
                  (if (p:stringp (cdr item))
                    (cdr item)
                    (vl-prin1-to-string (cdr item))))
                fp)))))
      (close fp)
      t)))
```
</details>

---

### ini:set

**说明**: 设置 ini 的某项的值。lst-ini ini文件的解析结果表, node 节 ，attr 属性项, value 值。

**用法**:
```lisp
(ini:set lst-ini node attr value)
```

**参数**: 1 lst-ini  : 列表;2 node  : 未识别定义;3 attr  : 未识别定义;4 value  : 值;

**返回值**: lst-ini

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ini:set (lst-ini node attr value / sub)
  "设置 ini 的某项的值。lst-ini ini文件的解析结果表, node 节 ，attr 属性项, value 值。"
  "lst-ini"
  (setq node (strcat "["
      (vl-string-trim "[] "
        node)
      "]"))
  (setq sub (cdr (assoc node lst-ini)))
  (setq sub (subst (cons attr value)
      (assoc attr sub)
      sub))
  (setq lst-ini (subst (cons node sub)
      (assoc node lst-ini)
      lst-ini)))
```
</details>

---

## json

**模块说明**: 专用功能模块

### json:decode-json-from-string

**说明**: 将json字符串解码成list

**用法**:
```lisp
(json:decode-json-from-string str)
```

**参数**: 1 str  : 字符串;

**返回值**: list

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun json:decode-json-from-string (str)
  "将json字符串解码成list"
  "list"
  (read (@::post (strcat (@::uri)
        "/api/decode-json")
      str)))
```
</details>

---

### json:encode-from-alist

**说明**: 将关联列表转化为json串,测试版

**用法**:
```lisp
(json:encode-from-alist lst)
```

**参数**: 1 lst  : 列表;

**返回值**: list

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun json:encode-from-alist (lst / lst2array)
  "将关联列表转化为json串,测试版"
  "list"
  (defun lst2array (lst)
    (strcat "["
      (string:from-list (mapcar (quote @:to-string)
          lst)
        ",")
      "]"))
  (cond ((and (listp lst)
        (not (null lst)))
      (cond ((null (vl-list-length lst))
          (strcat "{\""
            (@:to-string (car lst))
            "\"
            : \""
            (@:to-string (cdr lst))
            "\"}"))
        ((and (apply (quote and)
              (mapcar (quote listp)
                lst))
            (apply (quote and)
              (mapcar (quote (lambda (x)
                    (and (= 1 (length x))
                      (atom (car x)))))
                lst)))
          (lst2array (mapcar (quote car)
              lst)))
        ((= 1 (length lst))
          (if (listp (car lst))
            (json:encode-from-alist (car lst))
            (@:to-string (car lst))))
        ((and (>= (length lst)
              2)
            (apply (quote and)
              (mapcar (quote atom)
                lst)))
          (strcat "{\""
            (@:to-string (car lst))
            "\"
            :"
            (lst2array (cdr lst))
            "}"))
        (t (strcat "{"
            (string:from-list (mapcar (quote json:encode-from-alist)
                lst)
              ",")
            "}"))))
    ((atom lst)
      (strcat "
        "
        (@:to-string lst)
        "
        "))))
```
</details>

---

### json:encode-json-alist

**说明**: 将属性表（点对表组成的列表）编码为json字符串

**用法**:
```lisp
(json:encode-json-alist alist)
```

**参数**: 1 alist  : 未识别定义;

**返回值**: String

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun json:encode-json-alist (alist)
  "将属性表（点对表组成的列表）编码为json字符串"
  "String"
  (@::post (strcat (@::uri)
      "/api/encode-json")
    (vl-prin1-to-string alist)))
```
</details>

---

### json:parse

**说明**: Json 字符串转化为 lisp 列表。

**用法**:
```lisp
(json:parse str)
```

**参数**: 1 str  : 字符串;

**返回值**: list

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun json:parse (str)
  "Json 字符串转化为 lisp 列表。"
  "list"
  (setq lst-str (vl-string->list str))
  (setq flag-escape nil)
  (setq flag-quote nil)
  (setq flag-keylevel 0)
  (setq flag-arraylevel 0)
  (setq atom-str-lst (quote nil))
  (while lst-str (setq curr-char (car lst-str))
    (cond ((= (ascii "\\")
          CURR-CHAR)
        (SETQ FLAG-ESCAPE (NOT FLAG-ESCAPE))
        (SETQ ATOM-STR-LST (CONS CURR-CHAR ATOM-STR-LST)))
      ((AND (= (ASCII "\"")
            CURR-CHAR)
          (NULL FLAG-ESCAPE))
        (SETQ FLAG-ESCAPE nil)
        (SETQ FLAG-QUOTE (NOT FLAG-QUOTE))
        (SETQ ATOM-STR-LST (CONS CURR-CHAR ATOM-STR-LST)))
      (T (SETQ FLAG-ESCAPE nil)
        (IF FLAG-QUOTE (SETQ ATOM-STR-LST (CONS CURR-CHAR ATOM-STR-LST))
          (COND ((= (ASCII "{")
                CURR-CHAR)
              (SETQ FLAG-KEYLEVEL (1+ FLAG-KEYLEVEL))
              (SETQ ATOM-STR-LST (CONS (ASCII "(")
                    ATOM-STR-LST)))
              ((= (ASCII "[")
                  CURR-CHAR)
                (SETQ FLAG-ARRAYLEVEL (1+ FLAG-ARRAYLEVEL))
                (IF (= PRE-CHAR (ASCII ":"))
                  (SETQ ATOM-STR-LST (CONS (ASCII "(")
                        (CDDDR ATOM-STR-LST)))
                    (SETQ ATOM-STR-LST (CONS (ASCII "(")
                          ATOM-STR-LST))))
                  ((= (ASCII "}")
                      CURR-CHAR)
                    (SETQ FLAG-KEYLEVEL (1- FLAG-KEYLEVEL))
                    (SETQ ATOM-STR-LST (CONS (ASCII ")")
                      ATOM-STR-LST)))
                ((= (ASCII "]")
                    CURR-CHAR)
                  (SETQ FLAG-ARRAYLEVEL (1- FLAG-ARRAYLEVEL))
                  (SETQ ATOM-STR-LST (CONS (ASCII ")")
                    ATOM-STR-LST)))
              ((= (ASCII ":")
                  CURR-CHAR)
                (SETQ ATOM-STR-LST (CONS (ASCII "
                      ")
                    (CONS (ASCII ".")
                      (CONS (ASCII "
                          ")
                        ATOM-STR-LST)))))
              ((= (ASCII ",")
                  CURR-CHAR)
                (SETQ ATOM-STR-LST (CONS (ASCII "(")
                      (CONS (ASCII ")")
                      ATOM-STR-LST))))
              (T (SETQ ATOM-STR-LST (CONS CURR-CHAR ATOM-STR-LST)))))))
      (SETQ PRE-CHAR CURR-CHAR)
      (SETQ LST-STR (CDR LST-STR)))
    (READ (STRCAT "("
          (VL-LIST->STRING (REVERSE ATOM-STR-LST))
          ")")))
```
</details>

---

## layer

**模块说明**: 图层管理、属性设置

### layer:activelayer

**说明**: 设置指定层为当前层

**用法**:
```lisp
(layer:activelayer name)
```

**参数**: 1 name  : 未明确定义;

**返回值**: 成功返回t，失败返回nil

**示例**:
```lisp
(entity:ActiveLayer "layer1")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:activelayer (name / iloc out)
    "设置指定层为当前层"
    "成功返回t，失败返回nil"
    "(entity:activelayer \"layer1\")"
    (if (and (tblsearch "layer"
                name)
            (setq iloc (vl-position name (layer:list))))
        (progn (vla-put-activelayer (std:active-document)
                (vla-item (std:layers)
                    iloc))
            t)
        nil))
```
</details>

---

### layer:allname

**说明**: 返回所有图层的名称(字符串表)

**用法**:
```lisp
(layer:allname )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:allname (/ out)
    "返回所有图层的名称(字符串表)"
    (vlax-for obj *lays* (setq out (cons (vlax-get-property obj (quote name))
                out)))
    (reverse out))
```
</details>

---

### layer:delete

**说明**: 删除图层。参数支持名称，图元，vla-object以及它们的列表。删除前需清空图层中的图元。

**用法**:
```lisp
(layer:delete layers)
```

**参数**: 1 layers  : 未识别定义;

**返回值**: t or nil

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:delete (layers / each out)
  "删除图层。参数支持名称，图元，vla-object以及它们的列表。删除前需清空图层中的图元。"
  "t or nil"
  (if (not (listp layers))
    (setq layers (list layers)))
  (setq layers (mapcar (quote (lambda (x)
          (cond ((p:stringp x)
              (e2o (tblobjname "layer"
                  x)))
            ((p:enamep x)
              (e2o x))
            ((p:vlap x)
              x))))
      layers))
  (if (null (ssget "x"
        (list (cons 8 (string:from-list (mapcar (quote (lambda (x)
                    (vla-get-name x)))
                layers)
              ",")))))
    (apply (quote and)
      (mapcar vla-delete layers))))
```
</details>

---

### layer:ent

**说明**: 获获取指定图层的图元名

**用法**:
```lisp
(layer:ent name)
```

**参数**: 1 name  : 未明确定义;

**返回值**: 图元

**示例**:
```lisp
(layer:ent "0") --> <图元名: -64cb388>
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:ent (name)
    "获获取指定图层的图元名"
    "图元"
    "(layer:ent \"0\")
    --> <图元名: -64cb388>"
    (tblobjname "layer"
        name))
```
</details>

---

### layer:freeze

**说明**: 图层列表冻结开关函数

**用法**:
```lisp
(layer:freeze laylist bool-flag)
```

**参数**: 1 laylist  : 未明确定义;  2 bool-flag  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:freeze (laylist bool-flag)
    "图层列表冻结开关函数"
    (vlax-for each (std:layers)
        (if (member (vla-get-name each)
                (if (listp laylist)
                    laylist (list laylist)))
            (if (vlax-write-enabled-p each)
                (vla-put-freeze each (if bool-flag :vlax-true :vlax-false))))
        (vlax-release-object each)))
```
</details>

---

### layer:freezed-p

**说明**: 层是否冻结？

**用法**:
```lisp
(layer:freezed-p lname)
```

**参数**: 1 lname  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:freezed-p (lname / each)
    "层是否冻结？"
    (list:exist (entity:freezelist)
        lname))
```
</details>

---

### layer:freezelist

**说明**: 返回冻结图层列表

**用法**:
```lisp
(layer:freezelist )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:freezelist (/ each out)
    "返回冻结图层列表"
    (vlax-for each (std:layers)
        (if (= (vla-get-freeze each)
                :vlax-true)
            (setq out (cons (vla-get-name each)
                    out))))
    out)
```
</details>

---

### layer:info

**说明**: 返回所有图层的信息

**用法**:
```lisp
(layer:info )
```

**参数**: None

**返回值**: (("层名" 状态 颜色 "线型")……);  状态：1冻结图层 2新视口冻结图层 4锁定…(其他看帮助);  颜色：负值为隐藏图层;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:info (/ lst d e1 e2)
    "返回所有图层的信息"
    "((\"层名\"
            状态 颜色 \"线型\")……)\n状态：1冻结图层 2新视口冻结图层 4锁定…(其他看帮助)\n颜色：负值为隐藏图层\n"
    (while (setq d (tblnext "layer"
                (null d)))
        (setq lst (cons (mapcar (quote cdr)
                    (cdr d))
                lst)))
    (vl-sort lst (quote (lambda (e1 e2)
                (< (car e1)
                    (car e2))))))
```
</details>

---

### layer:layerofflist

**说明**: 返回关闭图层列表

**用法**:
```lisp
(layer:layerofflist )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:layerofflist (/ each out)
    "返回关闭图层列表"
    (vlax-for each (std:layers)
        (if (= (vla-get-layeron each)
                :vlax-false)
            (setq out (cons (vla-get-name each)
                    out))))
    out)
```
</details>

---

### layer:layers

**说明**: 返回图层列表 list

**用法**:
```lisp
(layer:layers )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:layers (/ sll layer)
  "返回图层列表 list"
  (setq layer nil)
  (setq sll (tblnext "layer"
      t))
  (while (/= sll nil)
    (setq sll (cdr (assoc 2 sll)))
    (setq layer (cons sll layer))
    (setq sll (tblnext "layer"
        nil)))
  (reverse layer))
```
</details>

---

### layer:list

**说明**: 返回图层列表 list

**用法**:
```lisp
(layer:list )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:list (/ sll layer)
    "返回图层列表 list"
    (setq layer nil)
    (setq sll (tblnext "layer"
            t))
    (while (/= sll nil)
        (setq sll (cdr (assoc 2 sll)))
        (setq layer (cons sll layer))
        (setq sll (tblnext "layer"
                nil)))
    (reverse layer))
```
</details>

---

### layer:lock

**说明**: 图层锁定开关函数

**用法**:
```lisp
(layer:lock laylist bool-flag)
```

**参数**: 1 laylist  : 未明确定义;  2 bool-flag  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:lock (laylist bool-flag)
    "图层锁定开关函数"
    (vlax-for each (std:layers)
        (if (member (vla-get-name each)
                (if (listp laylist)
                    laylist (list laylist)))
            (if (vlax-write-enabled-p each)
                (vla-put-lock each (if bool-flag :vlax-true :vlax-false))))
        (vlax-release-object each)))
```
</details>

---

### layer:locked-p

**说明**: 层是否锁定？

**用法**:
```lisp
(layer:locked-p lname)
```

**参数**: 1 lname  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:locked-p (lname / each)
    "层是否锁定？"
    (list:exist (entity:lockedlist)
        lname))
```
</details>

---

### layer:lockedlist

**说明**: 返回锁定图层列表

**用法**:
```lisp
(layer:lockedlist )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:lockedlist (/ each out)
    "返回锁定图层列表"
    (vlax-for each (std:layers)
        (if (= (vla-get-lock each)
                :vlax-true)
            (setq out (cons (vla-get-name each)
                    out))))
    out)
```
</details>

---

### layer:make

**说明**: 创建一个图层参数1:name:图层名称参数2:colour:颜色默认nil(7)参数3:linetype:线型默认nil(continuous)参数4:n70:标志位，默认nil(0)  标准标记（按位编码值）：  1 = 冻结图层，否则解冻图层  2 = 默认情况下在新视口中冻结图层  4 = 锁定图层  16 = 如果设置了此位，则表条目外部依赖于外部参照  32 = 如果同时设置了此位和位 16，则表明已成功融入了外部依赖的外部参照  64 = 如果设置了此位，则表明在上次编辑图形时，图形中至少有一个图元参照了表条目。  (此标志适用于 autocad 命令。大多数读取 dxf 文件的程序都可以忽略它，并且无需由写入 dxf 文件的程序对其进行设置)

**用法**:
```lisp
(layer:make name colour linetype flag)
```

**参数**: 1 name  : 未识别定义;2 colour  : 未识别定义;3 linetype  : 未识别定义;4 flag  : 未识别定义;

**示例**:
```lisp
(layer:make "ABC" nil nil nil)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:make (name colour linetype flag)
  "创建一个图层\n参数1:name:图层名称\n参数2:colour:颜色默认nil(7)\n参数3:linetype:线型默认nil(continuous)\n参数4:n70:标志位，默认nil(0)\n  标准标记（按位编码值）：\n  1 = 冻结图层，否则解冻图层\n  2 = 默认情况下在新视口中冻结图层\n  4 = 锁定图层\n  16 = 如果设置了此位，则表条目外部依赖于外部参照\n  32 = 如果同时设置了此位和位 16，则表明已成功融入了外部依赖的外部参照\n  64 = 如果设置了此位，则表明在上次编辑图形时，图形中至少有一个图元参照了表条目。\n  (此标志适用于 autocad 命令。大多数读取 dxf 文件的程序都可以忽略它，并且无需由写入 dxf 文件的程序对其进行设置)"
  ""
  "(layer:make \"ABC\"
    nil nil nil)"
  (or flag (setq flag 0))
  (or colour (setq colour 7))
  (or linetype (setq linetype "continuous"))
  (entmakex (list (cons 0 "layer")
      (cons 100 "AcDbSymbolTableRecord")
      (cons 100 "AcDbLayerTableRecord")
      (cons 2 name)
      (cons 70 flag)
      (cons 62 colour)
      (cons 6 linetype))))
```
</details>

---

### layer:obj-name

**说明**: 返回所有图层对应的对象名(大写)

**用法**:
```lisp
(layer:obj-name )
```

**参数**: None

**返回值**: ((图层名1 对象名1) (图层名2 对象名2)……)

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:obj-name (/ ob)
    "返回所有图层对应的对象名(大写)"
    "((图层名1 对象名1)
        (图层名2 对象名2)……)"
    (vlax-for each (vla-get-layers *doc*)
        (setq ob (cons (list (vla-get-name each)
                    each)
                ob)))
    ob)
```
</details>

---

### layer:off

**说明**: 关闭图层;  参数：图层名称表

**用法**:
```lisp
(layer:off laylist)
```

**参数**: 1 laylist  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:off (laylist)
    "关闭图层\n参数：图层名称表"
    (setq laylist (mapcar (quote strcase)
            laylist))
    (vlax-for each *lays* (if (member (strcase (vla-get-name each))
                laylist)
            (if (vlax-write-enabled-p each)
                (vla-put-layeron each :vlax-false)))
        (vlax-release-object each)))
```
</details>

---

### layer:on

**说明**: 图层列表开关函数

**用法**:
```lisp
(layer:on laylist bool-flag)
```

**参数**: 1 laylist  : 未明确定义;  2 bool-flag  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:on (laylist bool-flag)
    "图层列表开关函数"
    (vlax-for each (std:layers)
        (if (member (vla-get-name each)
                (if (listp laylist)
                    laylist (list laylist)))
            (if (vlax-write-enabled-p each)
                (vla-put-layeron each (if bool-flag :vlax-true :vlax-false))))
        (vlax-release-object each)))
```
</details>

---

### layer:plotable

**说明**: 设置指定图层(列表)不打印n参数1、图层列表n参数2、是否打印(t打印/nil不打印)

**用法**:
```lisp
(layer:plotable laylist on-off)
```

**参数**: 1 laylist  : 未识别定义;2 on-off  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:plotable (laylist on-off)
  "设置指定图层(列表)不打印\n参数1、图层列表\n参数2、是否打印(t打印/nil不打印)"
  (if (atom laylist)
    (setq laylist (list laylist)))
  (vlax-for each (vla-get-layers *doc*)
    (if (member (strcase (vla-get-name each))
        (mapcar (quote strcase)
          laylist))
      (if (vlax-write-enabled-p each)
        (if on-off (vla-put-plottable each :vlax-true)
          (vla-put-plottable each :vlax-false))))
    (vlax-release-object each)))
```
</details>

---

### layer:plottable

**说明**: 图层打印开关函数

**用法**:
```lisp
(layer:plottable laylist bool-flag)
```

**参数**: 1 laylist  : 未明确定义;  2 bool-flag  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:plottable (laylist bool-flag)
    "图层打印开关函数"
    (vlax-for each (std:layers)
        (if (member (vla-get-name each)
                (if (listp laylist)
                    laylist (list laylist)))
            (if (vlax-write-enabled-p each)
                (vla-put-plottable each (if bool-flag :vlax-true :vlax-false))))
        (vlax-release-object each)))
```
</details>

---

### layer:plottablelist

**说明**: 返回可打印图层列表

**用法**:
```lisp
(layer:plottablelist )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:plottablelist (/ each out)
    "返回可打印图层列表"
    (vlax-for each (std:layers)
        (if (= (vla-get-plottable each)
                :vlax-true)
            (setq out (cons (vla-get-name each)
                    out))))
    out)
```
</details>

---

### layer:readme

**说明**: 图层操作相关函数。

**用法**:
```lisp
(layer:readme )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:readme nil "图层操作相关函数。"
  (princ "图层操作相关函数。使用 (require 'layer:*)
    加载这些函数")
  (princ))
```
</details>

---

### layer:set-color

**说明**: 设置图层 lay 颜色号为 int，lay 支持文字及*号通配符，或图层实体类型，int 为 0~255 之间的整数

**用法**:
```lisp
(layer:set-color lay int)
```

**参数**: 1 lay  : 未识别定义;2 int  : 整数;

**返回值**: ename

**示例**:
```lisp
(layer:set-color "jz-*" 3)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layer:set-color (lay int / lst)
  "设置图层 lay 颜色号为 int，lay 支持文字及*号通配符，或图层实体类型，int 为 0~255 之间的整数"
  "ename"
  "(layer:set-color \"jz-*\"
    3)"
  (cond ((p:stringp lay)
      (setq lst (vl-remove-if-not (quote (lambda (x)
              (wcmatch (strcase x)
                (strcase lay))))
          (layer:list)))
      (foreach layername lst (setq %ent (entget (layer:ent layername)))
        (entmod (subst (cons 62 int)
            (assoc 62 %ent)
            %ent))))
    ((p:enamep lay)
      (setq %ent (entget lay))
      (entmod (subst (cons 62 int)
          (assoc 62 %ent)
          %ent)))))
```
</details>

---

## layout

**模块说明**: 专用功能模块

### layout:list

**说明**: 按照当前屏幕显示的顺序返回所有布局名称

**用法**:
```lisp
(layout:list )
```

**参数**: None

**返回值**: 布局名列表

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layout:list (/ a lst)
    "按照当前屏幕显示的顺序返回所有布局名称"
    "布局名列表"
    (vlax-for a *layouts* (setq lst (cons (list (vla-get-taborder a)
                    (vla-get-name a))
                lst)))
    (cdr (mapcar (quote cadr)
            (vl-sort lst (quote (lambda (x y)
                        (< (car x)
                            (car y))))))))
```
</details>

---

### layout:make-viewport

**说明**: 从模型空间生成布局

**用法**:
```lisp
(layout:make-viewport layout pt-center width height twistang pt-model)
```

**参数**: 1 layout  : 未识别定义;2 pt-center  : 单个2D/3D坐标点;3 width  : 未识别定义;4 height  : 未识别定义;5 twistang  : 未识别定义;6 pt-model  : 单个2D/3D坐标点;

**返回值**: ename

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layout:make-viewport (layout pt-center width height twistang pt-model / obj-pv)
  "从模型空间生成布局"
  "ename"
  (if (null (member layout (layout:list)))
    (progn (vla-add *layouts* layout)
      (if (/= layout (getvar "ctab"))
        (setvar "ctab"
          layout))
      (pickset:erase (ssget "x"
          (list (cons 410 layout)))))
    (if (/= layout (getvar "ctab"))
      (setvar "ctab"
        layout)))
  (if (null twistang)
    (setq twistang 0.0)
    (if (not (zerop twistang))
      (setq pt-model (m:coordinate-rotate pt-model twistang))))
  (if (/= layout (getvar "ctab"))
    (setvar "ctab"
      layout))
  (setvar "cmdecho"
    0)
  (@::cmd "mview"
    "NE"
    (polar (polar pt-model pi (* 0.5 width))
      (* 1.5 pi)
      (* 0.5 height))
    (polar (polar pt-model 0 (* 0.5 width))
      (* 0.5 pi)
      (* 0.5 height))
    ""
    pt-center)
  (setvar "cmdecho"
    1)
  (setq obj-pv (e2o (entlast)))
  (vla-put-width obj-pv width)
  (vla-put-height obj-pv height)
  (vla-put-customscale obj-pv 1.0)
  (vla-put-twistangle obj-pv twistang)
  (o2e obj-pv))
```
</details>

---

### layout:readme

**说明**: 布局操作相关函数。

**用法**:
```lisp
(layout:readme )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layout:readme nil "布局操作相关函数。"
  (princ "布局操作相关函数。使用 (require 'layout:*)
    加载这些函数")
  (princ))
```
</details>

---

### layout:rename

**说明**: 重命名布局名称

**用法**:
```lisp
(layout:rename str-oldname str-newname)
```

**参数**: 1 str-oldname  : 字符串;2 str-newname  : 字符串;

**示例**:
```lisp
(layout:rename "布局1" "我的布局")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layout:rename (str-oldname str-newname / lst-name)
  "重命名布局名称"
  ""
  "(layout:rename \"布局1\"
    \"我的布局\")"
  (setq lst-name (mapcar (quote vla-get-name)
      (layout:vla-list)))
  (if (and (member str-oldname lst-name)
      (null (member str-newname lst-name)))
    (vla-put-name (nth (vl-position str-oldname lst-name)
        (layout:vla-list))
      str-newname)))
```
</details>

---

### layout:set-position

**说明**: 根据指定布局名称修改布局的位置

**用法**:
```lisp
(layout:set-position name n)
```

**参数**: 1 name  : 未明确定义;  2 n  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layout:set-position (name n)
    "根据指定布局名称修改布局的位置"
    (setq vpnm1 (vla-item *layouts* name))
    (vla-put-taborder vpnm1 n))
```
</details>

---

### layout:sort

**说明**: 自动按布局名排序布局

**用法**:
```lisp
(layout:sort )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layout:sort (/ i layname layname-s)
    "自动按布局名排序布局"
    (setq layname (layout:list)
        layname-s (string:sort-by-numer layname)
        i 0)
    (foreach n layname-s (setq i (1+ i))
        (layout:set-position n i)))
```
</details>

---

### layout:vla-list

**说明**: 按照当前屏幕显示的顺序返回所有布局对象

**用法**:
```lisp
(layout:vla-list )
```

**参数**: None

**返回值**: 布局对象列表

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun layout:vla-list (/ a lst)
    "按照当前屏幕显示的顺序返回所有布局对象"
    "布局对象列表"
    (vlax-for a *layouts* (setq lst (cons (list (vla-get-taborder a)
                    a)
                lst)))
    (cdr (mapcar (quote cadr)
            (vl-sort lst (quote (lambda (x y)
                        (< (car x)
                            (car y))))))))
```
</details>

---

## line

**模块说明**: 专用功能模块

### line:get-lwpoints

**说明**: 生成多段线的点序

**用法**:
```lisp
(line:get-lwpoints en0)
```

**参数**: 1 en0  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun line:get-lwpoints (en0 / ddlist dd1 tmplist)
    "生成多段线的点序"
    (setq ddlist nil)
    (setq tmplist (entget en0))
    (repeat (cdr (assoc 90 (entget en0)))
        (setq dd1 (cdr (assoc 10 tmplist)))
        (setq tmplist (member (assoc 10 tmplist)
                tmplist))
        (setq tmplist (cdr tmplist))
        (setq ddlist (append ddlist (list dd1)))))
```
</details>

---

### line:length

**说明**: 求线段实体长度

**用法**:
```lisp
(line:length ent-line)
```

**参数**: 1 ent-line  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun line:length (ent-line)
    "求线段实体长度"
    (apply (quote distance)
        (entity:getdxf ent-line (quote (10 11)))))
```
</details>

---

### line:mid

**说明**: 求线段实体中点坐标

**用法**:
```lisp
(line:mid ent-line)
```

**参数**: 1 ent-line  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun line:mid (ent-line)
    "求线段实体中点坐标"
    (polar (entity:getdxf ent-line 10)
        (apply (quote angle)
            (entity:getdxf ent-line (quote (10 11))))
        (* 0.5 (line:length ent-line))))
```
</details>

---

## list

**模块说明**: 列表操作、排序、去重、查找

### list:+

**说明**: 两个列表各项相加之和组成的列表，列表长度以参数中列表长度小的为准.参数:lst1,lst2:数字列表

**用法**:
```lisp
(list:+ lst1 lst2)
```

**参数**: 1 lst1  : 列表;2 lst2  : 列表;

**返回值**: 列表各项相加后的列表

**示例**:
```lisp
(list:+ '(1 2) '(3 4))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:+ (lst1 lst2)
  "两个列表各项相加之和组成的列表，列表长度以参数中列表长度小的为准.\n参数:lst1,lst2:数字列表"
  "列表各项相加后的列表"
  "(list:+ '(1 2)
    '(3 4))"
  (mapcar (quote +)
    lst1 lst2))
```
</details>

---

### list:-

**说明**: 两个列表各项差组成的列表，列表长度以参数中列表长度小的为准

**用法**:
```lisp
(list:- lst1 lst2)
```

**参数**: 1 lst1  : 列表;  2 lst2  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:- (lst1 lst2)
    "两个列表各项差组成的列表，列表长度以参数中列表长度小的为准"
    (mapcar (quote -)
        lst1 lst2))
```
</details>

---

### list:add

**说明**: 两个列表各项相加之和组成的列表，列表长度以参数中列表长度小的为准.参数:lst1,lst2:数字列表

**用法**:
```lisp
(list:add lst1 lst2)
```

**参数**: 1 lst1  : 列表;2 lst2  : 列表;

**返回值**: 列表各项相加后的列表

**示例**:
```lisp
(list:+ '(1 2) '(3 4))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:add (lst1 lst2)
  "两个列表各项相加之和组成的列表，列表长度以参数中列表长度小的为准.\n参数:lst1,lst2:数字列表"
  "列表各项相加后的列表"
  "(list:+ '(1 2)
    '(3 4))"
  (mapcar (quote +)
    lst1 lst2))
```
</details>

---

### list:assoclist-additem

**说明**: 添加关联表的元素,无替换

**用法**:
```lisp
(list:assoclist-additem lst value)
```

**参数**: 1 lst  : 列表;  2 value  : 值;

**返回值**: 关联表，无相同的key

**示例**:
```lisp
(list:AssocList-AddItem '((1 11) (2 22) (3 33) (4 44)) '(2 33))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:assoclist-additem (lst value)
    "添加关联表的元素,无替换"
    "关联表，无相同的key"
    "(list:assoclist-additem '((1 11)
            (2 22)
            (3 33)
            (4 44))
        '(2 33))"
    (if (assoc (car value)
            lst)
        lst (cons value lst)))
```
</details>

---

### list:assoclist-appenditem

**说明**: 添加关联表的元素,替换. 同 assoc

**用法**:
```lisp
(list:assoclist-appenditem lst value)
```

**参数**: 1 lst  : 列表;  2 value  : 值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:assoclist-appenditem (lst value)
    "添加关联表的元素,替换. 同 assoc"
    (if (assoc (car value)
            lst)
        (setq lst (list:assoclist-remove lst (car value))))
    (cons value lst))
```
</details>

---

### list:assoclist-appendlist

**用法**:
```lisp
(list:assoclist-appendlist lst value)
```

**参数**: 1 lst  : 列表;  2 value  : 值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:assoclist-appendlist (lst value)
    (if (listp value)
        (foreach item value (setq lst (list:assoclist-appenditem lst item)))))
```
</details>

---

### list:assoclist-index

**说明**: 根据key查找关联表的索引

**用法**:
```lisp
(list:assoclist-index lst key)
```

**参数**: 1 lst  : 列表;  2 key  : 键，关键字;

**返回值**: 索引，从0开始

**示例**:
```lisp
(list:AssocList-Index '((1 11) (2 22) (3 33) (4 44)) 3) ==> 2
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:assoclist-index (lst key)
    "根据key查找关联表的索引"
    "索引，从0开始"
    "(list:assoclist-index '((1 11)
            (2 22)
            (3 33)
            (4 44))
        3)
    ==> 2"
    (vl-position key (mapcar (quote car)
            lst)))
```
</details>

---

### list:assoclist-key

**说明**: 返回关联表中key对应的value,等价于(cdr (assoc key value))

**用法**:
```lisp
(list:assoclist-key lst key)
```

**参数**: 1 lst  : 列表;  2 key  : 键，关键字;

**返回值**: key对应的value

**示例**:
```lisp
(list:AssocList-Key lst key)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:assoclist-key (lst key)
    "返回关联表中key对应的value,等价于(cdr (assoc key value))"
    "key对应的value"
    "(list:assoclist-key lst key)"
    (cdr (assoc key lst)))
```
</details>

---

### list:assoclist-keys

**说明**: 返回关联表的key值表

**用法**:
```lisp
(list:assoclist-keys lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:assoclist-keys (lst)
    "返回关联表的key值表"
    (mapcar (quote car)
        lst))
```
</details>

---

### list:assoclist-remove

**说明**: 删除表中关联表匹配到key的的子表

**用法**:
```lisp
(list:assoclist-remove lst key)
```

**参数**: 1 lst  : 列表;  2 key  : 键，关键字;

**返回值**: 删除元素后的表

**示例**:
```lisp
(list:AssocList-Remove '((1 11) (2 22) (3 33) (4 44)) 2) ==>((1 11) (3 33) (4 44))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:assoclist-remove (lst key)
    "删除表中关联表匹配到key的的子表"
    "删除元素后的表"
    "(list:assoclist-remove '((1 11)
            (2 22)
            (3 33)
            (4 44))
        2)
    ==>((1 11)
        (3 33)
        (4 44))"
    (vl-remove-if (quote (lambda (x)
                (equal (car x)
                    key)))
        lst))
```
</details>

---

### list:assoclist-values

**说明**: 返回关联表的value值表

**用法**:
```lisp
(list:assoclist-values lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:assoclist-values (lst)
    "返回关联表的value值表"
    (mapcar (quote cdr)
        lst))
```
</details>

---

### list:change-index

**说明**: 交换列表的m和n项，索引从0开始

**用法**:
```lisp
(list:change-index lst m n)
```

**参数**: 1 lst  : 列表;  2 m  : 未明确定义;  3 n  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:change-index (lst m n / t1 t2)
    "交换列表的m和n项，索引从0开始"
    (setq t1 (nth m lst)
        t2 (nth n lst))
    (list:replace-index (list:replace-index lst m t2)
        n t1))
```
</details>

---

### list:delnotsame

**说明**: 查找表中不重复元素。

**用法**:
```lisp
(list:delnotsame lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:delnotsame (lst)
    "查找表中不重复元素。"
    (m:intersect lst (list:same lst)))
```
</details>

---

### list:delsame

**说明**: 删除表中相同项目，保留第一次出现的位置（支持容差）

**用法**:
```lisp
(list:delsame lst fuzz)
```

**参数**: 1 lst  : 列表;2 fuzz  : 容差;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:delsame (lst fuzz)
  "删除表中相同项目，保留第一次出现的位置（支持容差）"
  (if lst (cons (car lst)
      (list:delsame (vl-remove-if (quote (lambda (x)
              (list:equal (car lst)
                x fuzz)))
          (cdr lst))
        fuzz))))
```
</details>

---

### list:delsame-all

**说明**: 删除表中所有重复的元素

**用法**:
```lisp
(list:delsame-all lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:delsame-all (lst / l2)
    "删除表中所有重复的元素"
    (foreach i (list:same lst)
        (setq lst (vl-remove i lst))))
```
</details>

---

### list:difference

**说明**: 求差集.

**用法**:
```lisp
(list:difference lst1 lst2)
```

**参数**: 1 lst1  : 列表;2 lst2  : 列表;

**返回值**: list

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:difference (lst1 lst2 / res)
  "求差集."
  "list"
  (foreach a lst1 (if (not (member a lst2))
      (setq res (cons a res))))
  res)
```
</details>

---

### list:dot->list

**说明**: 点表转普通表

**用法**:
```lisp
(list:dot->list lst)
```

**参数**: 1 lst  : 列表;

**返回值**: 普通表

**示例**:
```lisp
(list:dot->list '(1 2 3 . 4))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(DEFUN LIST:DOT->LIST (LST)
"dot list to common list"
"common list"
    "(list:dot->list '(1 2 3 . 4))"
    (IF (LISTP LST)
        LST (COND ((AND LST (LISTP LST))
                (CONS (CAR LST)
                    (LIST:DOT->LIST (CDR LST))))
            ((AND LST (NOT (LISTP LST)))
                (CONS LST (LIST:DOT->LIST nil)))
            (T nil))))
```
</details>

---

### list:equal

**说明**: 比较含有浮点数的原子或表

**用法**:
```lisp
(list:equal lst1 lst2 fuzz)
```

**参数**: 1 lst1  : 列表;2 lst2  : 列表;3 fuzz  : 容差;

**返回值**: t or nil

**示例**:
```lisp
(list:equal 5.3 5.3 0.01)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:equal (lst1 lst2 fuzz)
  "比较含有浮点数的原子或表"
  "t or nil"
  "(list:equal 5.3 5.3 0.01)"
  (cond ((and (numberp lst1)
        (numberp lst2))
      (equal lst1 lst2 fuzz))
    ((and (listp lst1)
        (listp lst2)
        (> (length lst1)
          0)
        (= (length lst1)
          (length lst2)))
      (apply (quote and)
        (mapcar (quote (lambda (a b)
              (list:equal a b fuzz)))
          lst1 lst2)))
    (t (equal lst1 lst2))))
```
</details>

---

### list:exist

**说明**: 判断item是否在列表内

**用法**:
```lisp
(list:exist lst item)
```

**参数**: 1 lst  : 列表;  2 item  : 项或项值;

**返回值**: 存在t，反之nil

**示例**:
```lisp
(list:exist '(1 2 3 4) 3)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:exist (lst item)
    "判断item是否在列表内"
    "存在t，反之nil"
    "(list:exist '(1 2 3 4)
        3)"
    (apply (quote or)
        (cons (vl-position item lst)
            (mapcar (quote (lambda (x)
                        (list:exist x item)))
                (vl-remove-if (quote atom)
                    lst)))))
```
</details>

---

### list:fill

**说明**: 对于元素个数小于 n 的 lst, 用element 补足

**用法**:
```lisp
(list:fill lst n element)
```

**参数**: 1 lst  : 列表;2 n  : 整数;3 element  : 未识别定义;

**返回值**: list

**示例**:
```lisp
(list:fill '(a b) 4 'c) => '(a b c c)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:fill (lst n element)
  "对于元素个数小于 n 的 lst, 用element 补足"
  "list"
  "(list:fill '(a b)
    4 'c)
  => '(a b c c)"
  (if (and (numberp n)
      (listp lst)
      (< (length lst)
        n))
    (repeat (- (fix n)
        (length lst))
      (setq lst (append lst (list element)))))
  lst)
```
</details>

---

### list:flatten

**说明**: 将多维列表展平为一维。单向箔。

**用法**:
```lisp
(list:flatten lst)
```

**参数**: 1 lst  : 列表;

**返回值**: list

**示例**:
```lisp
(list:flatten '(a (b c) (d (e))))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:flatten (lst / lst1)
  "将多维列表展平为一维。单向箔。"
  "list"
  "(list:flatten '(a (b c)
      (d (e))))"
  (foreach x lst (cond ((atom x)
        (setq lst1 (append lst1 (list x))))
      ((listp x)
        (setq lst1 (append lst1 (list:flatten x)))))))
```
</details>

---

### list:get-front-nth

**说明**: 返回前 n 个元素

**用法**:
```lisp
(list:get-front-nth n lst)
```

**参数**: 1 n  : 未明确定义;  2 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:get-front-nth (n lst)
    "返回前 n 个元素"
    (if (= n 0)
        nil (cons (car lst)
            (list:get-front-nth (1- n)
                (cdr lst)))))
```
</details>

---

### list:get-ubound

**说明**: 得到表的各维数长度，最多支持到三维

**用法**:
```lisp
(list:get-ubound lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:get-ubound (lst)
    "得到表的各维数长度，最多支持到三维"
    (if (atom lst)
        (quote (0 0 0))
        (list (vl-list-length lst)
            (apply (quote max)
                (mapcar (quote vl-list-length)
                    lst))
            (apply (quote max)
                (mapcar (quote vl-list-length)
                    (apply (quote append)
                        lst))))))
```
</details>

---

### list:group-by

**说明**: 对已排序的列表lst进行分组。fun为分组依据

**用法**:
```lisp
(list:group-by lst fun)
```

**参数**: 1 lst  : 列表;2 fun  : 未识别定义;

**返回值**: lst

**示例**:
```lisp
(list:group-by '(a a a b b c) '(lambda(x y)(= x y))) => ((a a a)(b b)(c))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:group-by (lst fun / res g)
  "对已排序的列表lst进行分组。fun为分组依据"
  "lst"
  "(list:group-by '(a a a b b c)
    '(lambda(x y)(= x y)))
  => ((a a a)(b b)(c))"
  (setq res nil)
  (setq g (cons (car lst)
      nil))
  (while (setq lst (cdr lst))
    (setq a% (car lst))
    (if ((eval fun)
        (car g)
        a%)
      (setq g (cons a% g))
      (progn (setq res (cons (reverse g)
            res))
        (setq g (cons a% nil)))))
  (if g (setq res (cons (reverse g)
        res)))
  (reverse res))
```
</details>

---

### list:indot->list

**说明**: 内嵌点表的表转普通表

**用法**:
```lisp
(list:indot->list lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:indot->list (lst / tmplist)
    "内嵌点表的表转普通表"
    (setq tmplist lst)
    (foreach i tmplist (if (dotpairp i)
            (setq lst (subst (list:dot->list i)
                    i lst))))
    lst)
```
</details>

---

### list:insert

**说明**: 在列表lst 的第 index 项前插入项 item。

**用法**:
```lisp
(list:insert lst index item)
```

**参数**: 1 lst  : 列表;  2 index  : 索引值;  3 item  : 项或项值;

**返回值**: 插入项后的列表

**示例**:
```lisp
(list:insert '(0 1 2 3) 1 5)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:insert (lst index item)
    "在列表lst 的第 index 项前插入项 item。"
    "插入项后的列表"
    "(list:insert '(0 1 2 3)
        1 5)"
    (if (zerop index)
        (cons item lst)
        (cons (car lst)
            (list:insert (cdr lst)
                (1- index)
                item))))
```
</details>

---

### list:insert-nth

**说明**: 插入元素va到lst表的第n位

**用法**:
```lisp
(list:insert-nth value n lst)
```

**参数**: 1 value  : 值;  2 n  : 未明确定义;  3 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:insert-nth (value n lst)
    "插入元素va到lst表的第n位"
    (if (= n 0)
        (cons value lst)
        (cons (car lst)
            (list:insert-nth value (1- n)
                (cdr lst)))))
```
</details>

---

### list:intersect

**说明**: 求两个列表集合的交集

**用法**:
```lisp
(list:intersect lst1 lst2)
```

**参数**: 1 lst1  : 列表;2 lst2  : 列表;

**返回值**: List

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:intersect (lst1 lst2 / res)
  "求两个列表集合的交集"
  "List"
  (if (null *fuzz*)
    (setq *fuzz* 0.0001))
  (foreach a lst2 (if (list:member a lst1 *fuzz*)
      (setq res (cons a res))))
  res)
```
</details>

---

### list:intersection

**说明**: 求两个列表的交集

**用法**:
```lisp
(list:intersection lst1 lst2)
```

**参数**: 1 lst1  : 列表;2 lst2  : 列表;

**返回值**: List

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:intersection (lst1 lst2 / res)
  "求两个列表的交集"
  "List"
  (foreach a lst2 (if (member a lst1)
      (setq res (cons a res))))
  res)
```
</details>

---

### list:item-num

**说明**: 表中元素及数量

**用法**:
```lisp
(list:item-num lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:item-num (lst / l2 tmp tmp1)
    "表中元素及数量"
    (while (setq l2 (cons (list (setq tmp1 (car lst))
                    (- (length lst)
                        (length (setq tmp (vl-remove tmp1 lst)))))
                l2)
            lst tmp))
    (reverse l2))
```
</details>

---

### list:ltrim

**说明**: 删除表头前m项

**用法**:
```lisp
(list:ltrim lst m)
```

**参数**: 1 lst  : 列表;  2 m  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:ltrim (lst m)
    "删除表头前m项"
    (cond ((or (zerop m)
                (minusp m)
                (>= m (length lst)))
            lst)
        (t (repeat m (setq lst (cdr lst))))))
```
</details>

---

### list:make-same

**说明**: 生成num个相同元素 element 的列表

**用法**:
```lisp
(list:make-same element num)
```

**参数**: 1 element  : 未识别定义;2 num  : 数值;

**返回值**: list

**示例**:
```lisp
(list:make-same 'a 5)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:make-same (element num / lst)
  "生成num个相同元素 element 的列表"
  "list"
  "(list:make-same 'a 5)"
  (repeat (fix num)
    (setq lst (cons element lst))))
```
</details>

---

### list:member

**说明**: 支持浮点数的member

**用法**:
```lisp
(list:member ele lst fuzz)
```

**参数**: 1 ele  : 未识别定义;2 lst  : 列表;3 fuzz  : 容差;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:member (ele lst fuzz)
  "支持浮点数的member"
  (if lst (cond ((list:equal ele (car lst)
          fuzz)
        lst)
      (t (list:member ele (cdr lst)
          fuzz)))))
```
</details>

---

### list:move

**说明**: 列表循环移动

**用法**:
```lisp
(list:move lst n)
```

**参数**: 1 lst  : 列表;  2 n  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:move (lst n)
    "列表循环移动"
    (repeat (abs n)
        (if (minusp n)
            (setq lst (append (list (last lst))
                    (list:rtrim lst 1)))
            (setq lst (append (cdr lst)
                    (list (car lst)))))))
```
</details>

---

### list:positions

**说明**: 获取元素 item 在 表 lst 中的所有位置。

**用法**:
```lisp
(list:positions item lst)
```

**参数**: 1 item  : 项或项值;2 lst  : 列表;

**返回值**: list

**示例**:
```lisp
(list:postions 'a '(a b a)) => (0 2)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:positions (item lst / res)
  "获取元素 item 在 表 lst 中的所有位置。"
  "list"
  "(list:postions 'a '(a b a))
  => (0 2)"
  (setq l (length lst))
  (while (setq lst (member item lst))
    (setq res (cons (- l (length lst))
        res))
    (setq lst (cdr lst)))
  (reverse res))
```
</details>

---

### list:range

**说明**: 生成等差数列表，类似python的range()函数.参数：start:起始值      end:结束值      step:等差值

**用法**:
```lisp
(list:range start end step)
```

**参数**: 1 start  : 未识别定义;2 end  : 单个图元;3 step  : 未识别定义;

**返回值**: 等差数列表

**示例**:
```lisp
(list:range 1 4 1);; => (1 2 3 4)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:range (start end step)
  "生成等差数列表，类似python的range()函数.\n参数：start:起始值\n      end:结束值\n      step:等差值"
  "等差数列表"
  "(list:range 1 4 1);; => (1 2 3 4)"
  (if (> start end)
    (quote nil)
    (cons start (list:range (+ start step)
        end step))))
```
</details>

---

### list:remove-duplicate-keys

**说明**: 删除点对表中重复的key，即子表中第一个元素唯一。

**用法**:
```lisp
(list:remove-duplicate-keys lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:remove-duplicate-keys (lst)
  "删除点对表中重复的key，即子表中第一个元素唯一。"
  (if lst (cons (car lst)
      (list:remove-duplicate-keys (vl-remove-if (quote (lambda (x)
              (eq (caar lst)
                (car x))))
          lst)))))
```
</details>

---

### list:remove-duplicates

**说明**: 删除列表中重复的原子。

**用法**:
```lisp
(list:remove-duplicates lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:remove-duplicates (lst)
    "删除列表中重复的原子。"
    (if lst (cons (car lst)
            (list:remove-duplicates (vl-remove (car lst)
                    lst)))))
```
</details>

---

### list:remove-front-nth

**说明**: 删除列表中表的前n个元素

**用法**:
```lisp
(list:remove-front-nth n lst)
```

**参数**: 1 n  : 未明确定义;  2 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:remove-front-nth (n lst)
    "删除列表中表的前n个元素"
    (if (= n 0)
        lst (list:remove-front-nth (1- n)
            (cdr lst))))
```
</details>

---

### list:remove-index

**说明**: 按索引删除列表的项,leemac

**用法**:
```lisp
(list:remove-index lst index)
```

**参数**: 1 lst  : 列表;  2 index  : 索引值;

**返回值**: 删除索引项之后的列表

**示例**:
```lisp
(list:RemoveIndex '(0 1 2 3) 1)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:remove-index (lst index / i)
    "按索引删除列表的项,leemac"
    "删除索引项之后的列表"
    "(list:removeindex '(0 1 2 3)
        1)"
    (setq i -1)
    (vl-remove-if (quote (lambda (x)
                (= (setq i (1+ i))
                    index)))
        lst))
```
</details>

---

### list:remove-nth

**说明**: 删除lst表的第n个元素

**用法**:
```lisp
(list:remove-nth n lst)
```

**参数**: 1 n  : 未明确定义;  2 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:remove-nth (n lst)
    "删除lst表的第n个元素"
    (if (= n 0)
        (cdr lst)
        (cons (car lst)
            (list:remove-nth (1- n)
                (cdr lst)))))
```
</details>

---

### list:remove-once

**说明**: 删除表中第一个匹配到的元素

**用法**:
```lisp
(list:remove-once lst item)
```

**参数**: 1 lst  : 列表;2 item  : 项或项值;

**返回值**: 删除元素后的表

**示例**:
```lisp
(list:remove-once '(1 2 3 4 3)        3)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:remove-once (lst item / f)
  "删除表中第一个匹配到的元素"
  "删除元素后的表"
  "(list:remove-once '(1 2 3 4 3)\n        3)"
  (setq f equal)
  (vl-remove-if (quote (lambda (a)
        (if (f a item)
          (setq f (lambda (a b)
              nil)))))
    lst))
```
</details>

---

### list:replace-index

**说明**: 按索引替换列表

**用法**:
```lisp
(list:replace-index oldlst index item)
```

**参数**: 1 oldlst  : 未明确定义;  2 index  : 索引值;  3 item  : 项或项值;

**返回值**: 替换后的列表

**示例**:
```lisp
(list:reeplace-index '(0 1 2 3) 1 5)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:replace-index (oldlst index item)
    "按索引替换列表"
    "替换后的列表"
    "(list:reeplace-index '(0 1 2 3)
        1 5)"
    (if (zerop index)
        (append (list item)
            (cdr oldlst))
        (cons (car oldlst)
            (list:replace-index (cdr oldlst)
                (1- index)
                item))))
```
</details>

---

### list:replace[m,n]

**说明**: 按索引替换列表第m子表的第n项

**用法**:
```lisp
(list:replace[m,n] oldlst m n item)
```

**参数**: 1 oldlst  : 未明确定义;  2 m  : 未明确定义;  3 n  : 未明确定义;  4 item  : 项或项值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:replace[m,n] (oldlst m n item)
    "按索引替换列表第m子表的第n项"
    (list:replace-index oldlst m (list:replace-index (nth m oldlst)
            n item)))
```
</details>

---

### list:rm-m2n

**说明**: 删除列表的第m至n项，索引值从0计算

**用法**:
```lisp
(list:rm-m2n lst m n)
```

**参数**: 1 lst  : 列表;  2 m  : 未明确定义;  3 n  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:rm-m2n (lst m n / len i)
    "删除列表的第m至n项，索引值从0计算"
    (cond ((< m 0)
            (setq m 0))
        ((> n (setq len (length lst)))
            (setq n len)))
    (setq i -1)
    (vl-remove-if (quote (lambda (x)
                (and (<= m (setq i (1+ i)))
                    (<= i n))))
        lst))
```
</details>

---

### list:rtrim

**说明**: 删除表尾m项

**用法**:
```lisp
(list:rtrim lst m)
```

**参数**: 1 lst  : 列表;  2 m  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:rtrim (lst m)
    "删除表尾m项"
    (reverse (list:ltrim (reverse lst)
            m)))
```
</details>

---

### list:same

**说明**: 查找表中重复元素

**用法**:
```lisp
(list:same lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:same (lst)
    "查找表中重复元素"
    (if lst (if (member (car lst)
                (cdr lst))
            (cons (car lst)
                (list:same (vl-remove (car lst)
                        (cdr lst))))
            (list:same (vl-remove (car lst)
                    (cdr lst))))))
```
</details>

---

### list:same-num

**说明**: 表中相同元素及数量

**用法**:
```lisp
(list:same-num lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:same-num (lst / l2 tmp)
    "表中相同元素及数量"
    (while (setq tmp (vl-remove (car lst)
                lst)
            l2 (if (member (car lst)
                    (cdr lst))
                (cons (list (car lst)
                        (- (length lst)
                            (length tmp)))
                    l2)
                l2)
            lst tmp))
    (reverse l2))
```
</details>

---

### list:search-index

**说明**: 以索引查找表中元素;  参数：;    lst:列表;    index:索引或者索引表

**用法**:
```lisp
(list:search-index lst index)
```

**参数**: 1 lst  : 列表;  2 index  : 索引值;

**返回值**: 查找到的元素组成的表

**示例**:
```lisp
(list:Search-Index '(1 2 3 4) 3)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:search-index (lst index)
    "以索引查找表中元素\n参数：\n  lst:列表\n  index:索引或者索引表"
    "查找到的元素组成的表"
    "(list:search-index '(1 2 3 4)
        3)"
    (mapcar (quote (lambda (x)
                (nth x lst)))
        (if (atom index)
            (list index)
            index)))
```
</details>

---

### list:search-item

**说明**: 查找表中元素的索引，索引从0开始

**用法**:
```lisp
(list:search-item lst item)
```

**参数**: 1 lst  : 列表;  2 item  : 项或项值;

**返回值**: 索引值表

**示例**:
```lisp
(list:search-item '(1 2 3 4) 3)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:search-item (lst item / i result)
    "查找表中元素的索引，索引从0开始"
    "索引值表"
    "(list:search-item '(1 2 3 4)
        3)"
    (setq i 0)
    (foreach x lst (if (= x item)
            (setq result (cons i result)))
        (setq i (1+ i)))
    (reverse result))
```
</details>

---

### list:set-nth

**说明**: 更新lst表的第n个元素为value

**用法**:
```lisp
(list:set-nth value n lst)
```

**参数**: 1 value  : 值;  2 n  : 未明确定义;  3 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:set-nth (value n lst)
    "更新lst表的第n个元素为value"
    (if (= n 0)
        (cons value (cdr lst))
        (cons (car lst)
            (list:set-nth value (1- n)
                (cdr lst)))))
```
</details>

---

### list:sort

**说明**: 无损排序，vl-sort 可能会删除相同的元素，导致结果列表内的个数小于原列表。list:sort 不会删除表内元素。fun:排序依据

**用法**:
```lisp
(list:sort lst fun)
```

**参数**: 1 lst  : 列表;2 fun  : 未识别定义;

**返回值**: lst

**示例**:
```lisp
(list:sort '(1 2 3 1 2) '<)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:sort (lst fun / res)
  "无损排序，vl-sort 可能会删除相同的元素，导致结果列表内的个数小于原列表。list:sort 不会删除表内元素。fun:排序依据"
  "lst"
  "(list:sort '(1 2 3 1 2)
    '<)"
  (setq res (cons (car lst)
      nil))
  (while (setq lst (cdr lst))
    (if ((eval fun)
        (car res)
        (car lst))
      (setq res (cons (car lst)
          res))
      (progn (setq tmp (cons (car res)
            nil))
        (while (and (> (length res)
              0)
            ((eval fun)
              (car lst)
              (car tmp)))
          (setq tmp (cons (car (setq res (cdr res)))
              tmp)))
        (setq res (append (reverse (cdr tmp))
            (list (car lst))
            res)))))
  (reverse res))
```
</details>

---

### list:split

**说明**: 列表切分,不足部分省略，此函数返回结果相对list:split-2d、list:split-3d两个特殊函数比较合理

**用法**:
```lisp
(list:split lst x)
```

**参数**: 1 lst  : 列表;  2 x  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:split (lst x / lst2)
    "列表切分,不足部分省略，此函数返回结果相对list:split-2d、list:split-3d两个特殊函数比较合理"
    (foreach n lst (if (and lst2 (/= x (length (car lst2))))
            (setq lst2 (cons (append (car lst2)
                        (list n))
                    (cdr lst2)))
            (setq lst2 (cons (list n)
                    lst2))))
    (reverse lst2))
```
</details>

---

### list:split-2d

**说明**: 列表按顺序切分为2元素表组成的表,不足部分用nil表示

**用法**:
```lisp
(list:split-2d lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:split-2d (lst)
    "列表按顺序切分为2元素表组成的表,不足部分用nil表示"
    (if lst (cons (list (car lst)
                (cadr lst))
            (list:split-2d (cddr lst)))))
```
</details>

---

### list:split-3d

**说明**: 列表按顺序切分为3元素表组成的表,不足部分用nil表示

**用法**:
```lisp
(list:split-3d lst)
```

**参数**: 1 lst  : 列表;

**返回值**: ((x x x )(x x x)...)

**示例**:
```lisp
(list:split-3d '(1 2 3 4))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:split-3d (lst)
    "列表按顺序切分为3元素表组成的表,不足部分用nil表示"
    "((x x x )(x x x)...)"
    "(list:split-3d '(1 2 3 4))"
    (if lst (cons (list (car lst)
                (cadr lst)
                (caddr lst))
            (list:split-3d (cdddr lst)))))
```
</details>

---

### list:split-by

**说明**: 按给定的条件拆分表

**用法**:
```lisp
(list:split-by lst fun)
```

**参数**: 1 lst  : 列表;2 fun  : 未识别定义;

**返回值**: 由lst组成的lst

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:split-by (lst fun / res lst-sub)
  "按给定的条件拆分表"
  "由lst组成的lst"
  ""
  (setq res nil)
  (setq lst-sub nil)
  (foreach sub% lst (if ((eval fun)
        sub%)
      (progn (setq res (cons (reverse lst-sub)
            res))
        (setq lst-sub (cons sub% nil)))
      (setq lst-sub (cons sub% lst-sub))))
  (if (car lst-sub)
    (setq res (cons (reverse lst-sub)
        res)))
  (reverse res))
```
</details>

---

### list:split-index

**说明**: 根据索引分割列表，索引从0开始

**用法**:
```lisp
(list:split-index lst index)
```

**参数**: 1 lst  : 列表;  2 index  : 索引值;

**返回值**: 索引前后元素组成的表，其中索引所指向的元素位于第二个子表的表头

**示例**:
```lisp
(list:split-index '(1 2 3 4) 2)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:split-index (lst index / result tmp)
    "根据索引分割列表，索引从0开始"
    "索引前后元素组成的表，其中索引所指向的元素位于第二个子表的表头"
    "(list:split-index '(1 2 3 4)
        2)"
    (setq tmp lst)
    (repeat index (setq result (cons (car tmp)
                result))
        (setq tmp (cdr tmp)))
    (list (reverse result)
        (list:ltrim lst index)))
```
</details>

---

### list:sublist

**说明**: 获取子列表,leemac

**用法**:
```lisp
(list:sublist lst idx len)
```

**参数**: 1 lst  : 列表;  2 idx  : 未明确定义;  3 len  : 未明确定义;

**返回值**: 子列表

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:sublist (lst idx len / rtn)
    "获取子列表,leemac"
    "子列表"
    (setq len (if len (min len (- (length lst)
                    idx))
            (- (length lst)
                idx))
        idx (+ idx len))
    (repeat len (setq rtn (cons (nth (setq idx (1- idx))
                    lst)
                rtn))))
```
</details>

---

### list:subsetp

**说明**: 检查sub是否为sup的子集

**用法**:
```lisp
(list:subsetp sub sup)
```

**参数**: 1 sub  : 未识别定义;2 sup  : 未识别定义;

**返回值**: t nil

**示例**:
```lisp
(list:subsetp sub sup) 检查sub是否为sup的子集
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:subsetp (sub sup)
  "检查sub是否为sup的子集"
  "t nil"
  "(list:subsetp sub sup)
  检查sub是否为sup的子集"
  (cond ((null sub)
      t)
    ((member (car sub)
        sup)
      (list:subsetp (cdr sub)
        sup))
    (t nil)))
```
</details>

---

### list:subst

**说明**: 置换表中指定位置的元素

**用法**:
```lisp
(list:subst n a l)
```

**参数**: 1 n  : 未明确定义;  2 a  : 未明确定义;  3 l  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:subst (n a l)
    "置换表中指定位置的元素"
    (cond ((numberp n)
            (if (zerop n)
                (append (list a)
                    (cdr l))
                (cons (car l)
                    (list:subst (1- n)
                        a (cdr l)))))
        ((listp n)
            (cond ((equal (length n)
                        1)
                    (if (zerop (car n))
                        (append (list a)
                            (cdr l))
                        (cons (car l)
                            (list:subst (1- (car n))
                                a (cdr l)))))
                ((> (length n)
                        1)
                    (if (zerop (car n))
                        (cons (list:subst (cdr n)
                                a (car l))
                            (cdr l))
                        (cons (car l)
                            (list:subst (append (list (1- (car n)))
                                    (cdr n))
                                a (cdr l)))))))))
```
</details>

---

### list:trim

**说明**: 删除表头前m项，表尾前n项

**用法**:
```lisp
(list:trim lst m n)
```

**参数**: 1 lst  : 列表;  2 m  : 未明确定义;  3 n  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:trim (lst m n)
    "删除表头前m项，表尾前n项"
    (list:ltrim (list:rtrim lst n)
        m))
```
</details>

---

### list:union

**说明**: 求两个集合的并集

**用法**:
```lisp
(list:union lst1 lst2)
```

**参数**: 1 lst1  : 列表;2 lst2  : 列表;

**返回值**: list

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun list:union (lst1 lst2 / lst)
  "求两个集合的并集"
  "list"
  (if (null *fuzz*)
    (setq *fuzz* 0.001))
  (setq lst lst1)
  (foreach a lst2 (if (not (list:member a lst *fuzz*))
      (setq lst (cons a lst))))
  lst)
```
</details>

---

## m

**模块说明**: 数学运算、三角函数

### m:acos

**说明**: 计算反余弦值

**用法**:
```lisp
(m:acos x)
```

**参数**: 1 x  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:acos (x)
    "计算反余弦值"
    (if (<= -1.0 x 1.0)
        (atan (sqrt (- 1.0 (* x x)))
            x)))
```
</details>

---

### m:arcosh

**说明**: 计算反双曲余弦值

**用法**:
```lisp
(m:arcosh x)
```

**参数**: 1 x  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:arcosh (x)
    "计算反双曲余弦值"
    (if (<= 1.0 x)
        (log (+ x (sqrt (1- (* x x)))))))
```
</details>

---

### m:arsinh

**说明**: 计算反双曲正弦值

**用法**:
```lisp
(m:arsinh x)
```

**参数**: 1 x  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:arsinh (x)
    "计算反双曲正弦值"
    (log (+ x (sqrt (1+ (* x x))))))
```
</details>

---

### m:artanh

**说明**: 计算反双曲正切值

**用法**:
```lisp
(m:artanh x)
```

**参数**: 1 x  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:artanh (x)
    "计算反双曲正切值"
    (if (< (abs x)
            1.0)
        (/ (log (/ (1+ x)
                    (- 1.0 x)))
            2.0)))
```
</details>

---

### m:asin

**说明**: 计算反正弦值

**用法**:
```lisp
(m:asin x)
```

**参数**: 1 x  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:asin (x)
    "计算反正弦值"
    (if (<= -1.0 x 1.0)
        (atan x (sqrt (- 1.0 (* x x))))))
```
</details>

---

### m:azimuth

**说明**: 计算某个角度(以x轴正向，逆时针)的方位角(以Y轴正向，顺时针)

**用法**:
```lisp
(m:azimuth ang)
```

**参数**: 1 ang  : 角度值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:azimuth (ang)
    "计算某个角度(以x轴正向，逆时针)的方位角(以y轴正向，顺时针)"
    (cond ((= ang 0)
            (setq ang (* 0.5 pi)))
        ((= ang (* 0.5 pi))
            (setq ang 0))
        ((= ang pi)
            (setq ang (* 1.5 pi)))
        ((= ang (* 1.5 pi))
            (setq ang pi))
        ((= ang (* 2 pi))
            (setq ang 0))
        ((and (< ang (* 0.5 pi))
                (> ang 0))
            (setq ang (- (* 0.5 pi)
                    ang)))
        (t (setq ang (- (* 2.5 pi)
                    ang)))))
```
</details>

---

### m:base->base

**说明**: 进制转换

**用法**:
```lisp
(m:base->base n b1 b2)
```

**参数**: 1 n  : 未明确定义;  2 b1  : 未明确定义;  3 b2  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:base->base (n b1 b2)
    "进制转换"
    (m:dec->base (m:base->dec n b1)
        b2))
```
</details>

---

### m:base->dec

**说明**: 进制转换

**用法**:
```lisp
(m:base->dec n b)
```

**参数**: 1 n  : 未明确定义;  2 b  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:base->dec (n b / l)
    "进制转换"
    (if (= 1 (setq l (strlen n)))
        (- (ascii n)
            (if (< (ascii n)
                    65)
                48 55))
        (+ (* b (m:base->dec (substr n 1 (1- l))
                    b))
            (m:base->dec (substr n l)
                b))))
```
</details>

---

### m:base2dec

**说明**: 进制转换,strnum 字符串表示的数, int-b 进制(2-36)

**用法**:
```lisp
(m:base2dec strnum int-b)
```

**参数**: 1 strnum  : 字符串;2 int-b  : 整数;

**返回值**: fixnum

**示例**:
```lisp
(m:base2dec "B0A1" 16)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:base2dec (strnum int-b / l)
  "进制转换,strnum 字符串表示的数, int-b 进制(2-36)"
  "fixnum"
  "(m:base2dec \"B0A1\"
    16)"
  (setq strnum (strcase strnum))
  (if (= 1 (setq l (strlen strnum)))
    (- (ascii strnum)
      (if (< (ascii strnum)
          65)
        48 55))
    (+ (* (float int-b)
        (m:base2dec (substr strnum 1 (1- l))
          (float int-b)))
      (m:base2dec (substr strnum l)
        (float int-b)))))
```
</details>

---

### m:cal

**说明**: 根据给定表达式计算结果

**用法**:
```lisp
(m:cal lst1 lst2 str)
```

**参数**: 1 lst1  : 列表;  2 lst2  : 列表;  3 str  : 字符串;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:cal (lst1 lst2 str)
    "根据给定表达式计算结果"
    (if (not (list:exist (arx)
                "geomcal.arx"))
        (arxload "geomcal"
            "\n加载geomcal失败！"))
    (mapcar (quote set)
        lst1 lst2)
    (if (vl-every (quote (lambda (x)
                    (= (quote real)
                        (type x))))
            lst2)
        (cal (strcat "1.0*("
                    str ")"))
        (cal str)))
```
</details>

---

### m:calheight

**说明**: 目标点的高程

**用法**:
```lisp
(m:calheight pt1 pt2 podu)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;  2 pt2  : 单个2D/3D坐标点;  3 podu  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:calheight (pt1 pt2 podu)
    "目标点的高程"
    (subst (+ (caddr pt1)
            (* podu (distance (geometry:point-3d->2d pt1)
                    (geometry:point-3d->2d pt2))))
        (caddr pt2)
        pt2))
```
</details>

---

### m:coord-chg

**用法**:
```lisp
(m:coord-chg pt-wcs o-ucs o-ang)
```

**参数**: 1 pt-wcs  : 单个2D/3D坐标点;  2 o-ucs  : 未明确定义;  3 o-ang  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:coord-chg (pt-wcs o-ucs o-ang / pt1a x y z)
    (setq pt1a (mapcar (quote -)
            pt-wcs o-ucs))
    (setq x (car pt1a))
    (setq y (cadr pt1a))
    (list (+ (* x (car o-ang))
            (* y (cadr o-ang)))
        (- (* y (car o-ang))
            (* x (cadr o-ang)))
        0))
```
</details>

---

### m:coordinate

**说明**: 坐标向量变换

**用法**:
```lisp
(m:coordinate p-base point2d)
```

**参数**: 1 p-base  : 未明确定义;  2 point2d  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:coordinate (p-base point2d / x y z)
    "坐标向量变换"
    (setq x (car point2d))
    (setq y (cadr point2d))
    (list (+ (car p-base)
            x)
        (+ (cadr p-base)
            y)
        (last p-base)))
```
</details>

---

### m:coordinate-rotate

**说明**: 坐标旋转

**用法**:
```lisp
(m:coordinate-rotate point2d angle1)
```

**参数**: 1 point2d  : 未明确定义;  2 angle1  : 角度值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:coordinate-rotate (point2d angle1 / x y)
    "坐标旋转"
    (setq x (car point2d))
    (setq y (cadr point2d))
    (list (- (* x (cos angle1))
            (* y (sin angle1)))
        (+ (* x (sin angle1))
            (* y (cos angle1)))))
```
</details>

---

### m:coordinate-scale

**说明**: 坐标缩放

**用法**:
```lisp
(m:coordinate-scale point scale)
```

**参数**: 1 point  : 未明确定义;  2 scale  : 比例值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:coordinate-scale (point scale)
    "坐标缩放"
    (mapcar (quote (lambda (a)
                (* scale a)))
        point))
```
</details>

---

### m:cosh

**说明**: 计算双曲余弦值

**用法**:
```lisp
(m:cosh x)
```

**参数**: 1 x  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:cosh (x)
    "计算双曲余弦值"
    (/ (+ (exp x)
            (exp (- x)))
        2.0))
```
</details>

---

### m:dec->base

**说明**: 进制转换

**用法**:
```lisp
(m:dec->base n b)
```

**参数**: 1 n  : 未明确定义;  2 b  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:dec->base (n b)
    "进制转换"
    (if (< n b)
        (chr (+ n (if (< n 10)
                    48 55)))
        (strcat (m:dec->base (/ n b)
                b)
            (m:dec->base (rem n b)
                b))))
```
</details>

---

### m:dec2base

**说明**: 进制转换,fixnum 整数值, b 进制

**用法**:
```lisp
(m:dec2base fixnum b)
```

**参数**: 1 fixnum  : 未识别定义;2 b  : 未识别定义;

**返回值**: string

**示例**:
```lisp
(m:dec2base 3323 16)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:dec2base (fixnum b)
  "进制转换,fixnum 整数值, b 进制"
  "string"
  "(m:dec2base 3323 16)"
  (if (< fixnum b)
    (chr (+ fixnum (if (< fixnum 10)
          48 55)))
    (strcat (m:dec2base (/ fixnum b)
        b)
      (m:dec2base (rem fixnum b)
        b))))
```
</details>

---

### m:dec2hex

**说明**: 将十进制整数转换为16进制符号 0XAB的形式

**用法**:
```lisp
(m:dec2hex fixnum)
```

**参数**: 1 fixnum  : 未识别定义;

**返回值**: symbol

**示例**:
```lisp
(m:dec2hex 45217) ;; => 0xB0A1
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:dec2hex (fixnum)
  "将十进制整数转换为16进制符号 0XAB的形式"
  "symbol"
  "(m:dec2hex 45217)
  ;; => 0xB0A1"
  (read (strcat "0x"
      (m:dec->base fixnum 16))))
```
</details>

---

### m:degress->radions

**说明**: 角度转弧度函数

**用法**:
```lisp
(m:degress->radions degress)
```

**参数**: 1 degress  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:degress->radions (degress)
    "角度转弧度函数"
    (if (numberp degress)
        (* pi (/ degress 180.0))))
```
</details>

---

### m:difference

**说明**: 列表差集

**用法**:
```lisp
(m:difference lst1 lst2)
```

**参数**: 1 lst1  : 列表;  2 lst2  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:difference (lst1 lst2 / lst)
    "列表差集"
    (vl-remove-if (quote (lambda (x)
                (member x lst2)))
        lst1))
```
</details>

---

### m:dmm

**说明**: 根据给定弧度返回度分秒格式的表

**用法**:
```lisp
(m:dmm ang)
```

**参数**: 1 ang  : 角度值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:dmm (ang)
    "根据给定弧度返回度分秒格式的表"
    (vl-remove-if (quote (lambda (x)
                (= ""
                    x)))
        (string:parse-by-lst (angtos ang 1 4)
            (quote ("d"
                    "'"
                    "\"")))))
```
</details>

---

### m:dms

**说明**: 根据给定十进制角度返回度分秒格式的表

**用法**:
```lisp
(m:dms degress)
```

**参数**: 1 degress  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:dms (degress / d x m s)
    "根据给定十进制角度返回度分秒格式的表"
    (setq d (fix degress))
    (setq x (* (- degress d)
            60))
    (setq m (fix x))
    (setq s (* (- x m)
            60))
    (list d m s))
```
</details>

---

### m:evenp

**说明**: 测试一个整数是否为偶数

**用法**:
```lisp
(m:evenp n)
```

**参数**: 1 n  : 未识别定义;

**返回值**: T or nil

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:evenp (n)
  "测试一个整数是否为偶数"
  "T or nil"
  (= 0 (rem (fix n)
      2)))
```
</details>

---

### m:expmod

**用法**:
```lisp
(m:expmod base exp1 m)
```

**参数**: 1 base  : 未识别定义;2 exp1  : 未识别定义;3 m  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:expmod (base exp1 m)
  (setq base (float base))
  (setq exp1 (float exp1))
  (cond ((zerop exp1)
      1)
    ((m:evenp exp1)
      (rem (m:square (m:expmod base (/ (float exp1)
              2)
            m))
        m))
    (t (rem (* base (m:expmod base (- exp1 1)
            m))
        m))))
```
</details>

---

### m:factorial

**说明**: 求n 的阶乘。斯特林公式法。;

**用法**:
```lisp
(m:factorial n)
```

**参数**: 1 n  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:factorial (n)
    "求n 的阶乘。斯特林公式法。\n"
    (* (sqrt (* 2 pi n))
        (expt (/ n (exp 1))
            n)))
```
</details>

---

### m:fast-expt

**用法**:
```lisp
(m:fast-expt b n)
```

**参数**: 1 b  : 未识别定义;2 n  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:fast-expt (b n)
  (cond ((zerop n)
      1)
    ((m:evenp n)
      (m:square (fast-expt b (/ n 2))))
    (t (* b (fast-expt b (1- n))))))
```
</details>

---

### m:fermat-test

**说明**: 素数测试

**用法**:
```lisp
(m:fermat-test n)
```

**参数**: 1 n  : 未识别定义;

**返回值**: T or nil

**示例**:
```lisp
(m:fermat-test 11)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:fermat-test (n / test-it)
  "素数测试"
  "T or nil"
  "(m:fermat-test 11)"
  (defun test-it (a)
    (= (m:expmod a n n)
      a))
  (test-it (+ 1 (fix (m:random (- n 1))))))
```
</details>

---

### m:fix-angle

**说明**: 使弧度值在 0-2pi 之间。

**用法**:
```lisp
(m:fix-angle angle0)
```

**参数**: 1 angle0  : 角度值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:fix-angle (angle0)
  "使弧度值在 0-2pi 之间。"
  (while (< angle0 0)
    (setq angle0 (+ pi pi angle0)))
  (while (>= angle0 (* pi 2))
    (setq angle0 (- angle0 (* pi 2))))
  angle0)
```
</details>

---

### m:gcd

**说明**: 求最大公约数

**用法**:
```lisp
(m:gcd a b)
```

**参数**: 1 a  : 未识别定义;2 b  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:gcd (a b)
  "求最大公约数"
  (if (= b 0)
    a (gcd b (rem a b))))
```
</details>

---

### m:hex2dec

**说明**: 16进制转换为10进制整数,hex 为 0x 开头的符号或字符串。

**用法**:
```lisp
(m:hex2dec hex)
```

**参数**: 1 hex  : 未识别定义;

**返回值**: fixnum

**示例**:
```lisp
(m:hex2dec '0xb0a1) ;;=> 45217
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:hex2dec (hex / l)
  "16进制转换为10进制整数,hex 为 0x 开头的符号或字符串。"
  "fixnum"
  "(m:hex2dec '0xb0a1)
  ;;=> 45217"
  (if (= (quote sym)
      (type hex))
    (setq hex (vl-symbol-name hex)))
  (setq hex (vl-string-left-trim "0xX"
      hex))
  (setq hex (strcase hex))
  (m:base2dec hex 16))
```
</details>

---

### m:intersect

**说明**: 列表交集

**用法**:
```lisp
(m:intersect lst1 lst2)
```

**参数**: 1 lst1  : 列表;  2 lst2  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:intersect (lst1 lst2)
    "列表交集"
    (vl-remove-if-not (quote (lambda (x)
                (member x lst2)))
        lst1))
```
</details>

---

### m:length

**说明**: 两点长度(距离)，等同于两点向量的模

**用法**:
```lisp
(m:length start end)
```

**参数**: 1 start  : 未明确定义;  2 end  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:length (start end)
    "两点长度(距离)，等同于两点向量的模"
    (vector:norm (mapcar (quote -)
            end start)))
```
</details>

---

### m:linear-interpolation

**说明**: 线性插值 y=y1+(y2-y1)*(x-x1)/(x2-x1)

**用法**:
```lisp
(m:linear-interpolation x x1 x2 y1 y2)
```

**参数**: 1 x  : 未识别定义;2 x1  : 未识别定义;3 x2  : 未识别定义;4 y1  : 未识别定义;5 y2  : 未识别定义;

**返回值**: number

**示例**:
```lisp
(m:liner-inerpolation 3.2 3 4 5 6)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:linear-interpolation (x x1 x2 y1 y2)
  "线性插值 y=y1+(y2-y1)*(x-x1)/(x2-x1)"
  "number"
  "(m:liner-inerpolation 3.2 3 4 5 6)"
  (+ y1 (* (- y2 y1)
      (/ (float (- x x1))
        (float (- x2 x1))))))
```
</details>

---

### m:maxlist

**说明**: 返回数值列表的中的最大值

**用法**:
```lisp
(m:maxlist lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:maxlist (lst)
    "返回数值列表的中的最大值"
    (if (atom lst)
        lst (apply (quote max)
            lst)))
```
</details>

---

### m:mid

**说明**: 计算中点

**用法**:
```lisp
(m:mid x y)
```

**参数**: 1 x  : 未明确定义;  2 y  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:mid (x y / a b)
    "计算中点"
    (mapcar (quote (lambda (a b)
                (* (+ a b)
                    0.5)))
        x y))
```
</details>

---

### m:minlist

**说明**: 返回数值列表的中的最小值

**用法**:
```lisp
(m:minlist lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:minlist (lst)
    "返回数值列表的中的最小值"
    (if (atom lst)
        lst (apply (quote min)
            lst)))
```
</details>

---

### m:mulmod

**说明**: 快速积求模

**用法**:
```lisp
(m:mulmod a b m)
```

**参数**: 1 a  : 未明确定义;  2 b  : 未明确定义;  3 m  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:mulmod (a b m / ret)
    "快速积求模"
    (setq ret 0)
    (while (not (zerop b))
        (if (not (zerop (rem b 2)))
            (setq ret (rem (+ a ret)
                    m)))
        (setq b (fix (/ b 2))
            a (rem (* a 2.0)
                m)))
    (fix ret))
```
</details>

---

### m:power

**说明**: 增强power函数，目的为扩展expt函数,参数都为数字时，字符串，数字，列表类型，其他类型返回nil,返回expt计算的结果，base为字符串和列表时，返回自乘的结果

**用法**:
```lisp
(m:power base pow)
```

**参数**: 1 base  : 未明确定义;  2 pow  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:power (base pow / str1)
    "增强power函数，目的为扩展expt函数,参数都为数字时，字符串，数字，列表类型，其他类型返回nil,返回expt计算的结果，base为字符串和列表时，返回自乘的结果"
    (cond ((stringp base)
            (progn (setq str1 "")
                (repeat pow (setq str1 (strcat str1 base)))))
        ((or (intp base)
                (realp base))
            (expt base pow))
        ((listp base)
            (progn (repeat pow (setq str1 (cons base str1)))))
        (t nil)))
```
</details>

---

### m:radions->degress

**说明**: 弧度转角度函数

**用法**:
```lisp
(m:radions->degress radions)
```

**参数**: 1 radions  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:radions->degress (radions)
    "弧度转角度函数"
    (if (numberp radions)
        (* radions (/ 180.0 pi))))
```
</details>

---

### m:rand

**说明**: 生成伪随机数

**用法**:
```lisp
(m:rand )
```

**参数**: None

**返回值**: 0~1之间的实数

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:rand (/ a c m $xn)
  "生成伪随机数"
  "0~1之间的实数"
  (setq m 4.29497e+09 a 1.66453e+06 c 1.0139e+09 $xn (rem (+ c (* a (cond ($xn)
            ((getvar (quote date))))))
      m))
  (/ $xn m))
```
</details>

---

### m:rand-by-cputicks

**说明**: 以日期及CPUticks为参数生成随机数

**用法**:
```lisp
(m:rand-by-cputicks )
```

**参数**: None

**返回值**: 1~100之间的整数

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:rand-by-cputicks (/ n3 n4)
  "以日期及CPUticks为参数生成随机数"
  "1~100之间的整数"
  (setq n3 (rtos (rem (getvar "date")
        1)
      2 16))
  (setq n3 (substr n3 18 1)
    n3 (atoi n3))
  (setq n4 (rem (getvar "cputicks")
      100))
  (fix (rem (+ n3 n4)
      100)))
```
</details>

---

### m:random

**说明**: 生成伪随机数

**用法**:
```lisp
(m:random n)
```

**参数**: 1 n  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:random (n / a c m)
    "生成伪随机数"
    (setq m 4.29497e+009 a 1.66453e+006 c 1.0139e+009 $xn (rem (+ c (* a (cond ($xn)
                        ((getvar (quote date))))))
            m))
    (* n (/ $xn m)))
```
</details>

---

### m:random-fix

**用法**:
```lisp
(m:random-fix n m)
```

**参数**: 1 n  : 未明确定义;  2 m  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:random-fix (n m)
    (fix (+ n (rem (getvar "cputicks")
                (- m n -1)))))
```
</details>

---

### m:randrange

**说明**: 计算给定范围内的随机数

**用法**:
```lisp
(m:randrange a b)
```

**参数**: 1 a  : 未明确定义;  2 b  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:randrange (a b)
    "计算给定范围内的随机数"
    (+ (min a b)
        (fix (* (m:rand)
                (1+ (abs (- a b)))))))
```
</details>

---

### m:rtos

**说明**: 保留小数位数(四舍五入)

**用法**:
```lisp
(m:rtos real prec)
```

**参数**: 1 real  : 实数;  2 prec  : 未明确定义;

**返回值**: 四舍五入后的字符串

**示例**:
```lisp
(m:rtos 1.8000 3)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:rtos (real prec / dimzin result)
    "保留小数位数(四舍五入)"
    "四舍五入后的字符串"
    "(m:rtos 1.8000 3)"
    (setq dimzin (getvar (quote dimzin)))
    (setvar (quote dimzin)
        0)
    (setq result (vl-catch-all-apply (quote rtos)
            (list real 2 prec)))
    (setvar (quote dimzin)
        dimzin)
    (if (not (vl-catch-all-error-p result))
        result))
```
</details>

---

### m:sign

**说明**: 实数的符号(正负号)

**用法**:
```lisp
(m:sign x)
```

**参数**: 1 x  : 未明确定义;

**返回值**: t 为正，nil 为负

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:sign (x)
  "实数的符号(正负号)"
  "t 为正，nil 为负"
  (cond ((> x 0)
      -1.0)
    ((< x 0)
      1.0)
    (t 0)))
```
</details>

---

### m:sinh

**说明**: 计算双曲正弦值

**用法**:
```lisp
(m:sinh x)
```

**参数**: 1 x  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:sinh (x)
    "计算双曲正弦值"
    (/ (- (exp x)
            (exp (- x)))
        2.0))
```
</details>

---

### m:sort-by-curve

**说明**: 函数说明:沿曲线排序~%返 回 值:排序后的点表

**用法**:
```lisp
(m:sort-by-curve curve lst)
```

**参数**: 1 curve  : 曲线;  2 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:sort-by-curve (curve lst)
    "函数说明:沿曲线排序~%返 回 值:排序后的点表"
    (vl-sort lst (function (lambda (p1 p2 / m n)
                (setq m (vlax-curve-getclosestpointto curve p1 t))
                (setq n (vlax-curve-getclosestpointto curve p2 t))
                (cond ((< (vlax-curve-getdistatpoint curve m)
                            (vlax-curve-getdistatpoint curve n)))
                    ((equal m n 1.0e-008)
                        (< (distance p1 m)
                            (distance p2 n))))))))
```
</details>

---

### m:square

**用法**:
```lisp
(m:square x)
```

**参数**: 1 x  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:square (x)
    (* x (float x)))
```
</details>

---

### m:symmetric-difference

**说明**: 列表对称差集

**用法**:
```lisp
(m:symmetric-difference l1 l2)
```

**参数**: 1 l1  : 未明确定义;  2 l2  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:symmetric-difference (l1 l2)
    "列表对称差集"
    (append (vl-remove-if (quote (lambda (x)
                    (member x l2)))
            l1)
        (vl-remove-if (quote (lambda (x)
                    (member x l1)))
            l2)))
```
</details>

---

### m:tan

**说明**: 计算正切值

**用法**:
```lisp
(m:tan num)
```

**参数**: 1 num  : 数值;

**返回值**: 实数

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:tan (num)
  "计算正切值"
  "实数"
  (if (not (equal 0.0 (cos num)
        1.0e-10))
    (/ (sin num)
      (cos num))))
```
</details>

---

### m:tanh

**说明**: 计算双曲正切值

**用法**:
```lisp
(m:tanh x)
```

**参数**: 1 x  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:tanh (x)
    "计算双曲正切值"
    (/ (m:sinh x)
        (m:cosh x)))
```
</details>

---

### m:transpt

**说明**: 根据已知世界坐标和用户坐标的基准点，计算世界坐标对应的用户坐标

**用法**:
```lisp
(m:transpt base usrpt transpt ang)
```

**参数**: 1 base  : 未明确定义;  2 usrpt  : 未明确定义;  3 transpt  : 未明确定义;  4 ang  : 角度值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:transpt (base usrpt transpt ang)
    "根据已知世界坐标和用户坐标的基准点，计算世界坐标对应的用户坐标"
    (car (geometry:rotatebymatrix (geometry:translatebymatrix (list transpt)
                base usrpt)
            usrpt (- ang))))
```
</details>

---

### m:trim

**说明**: 数值后续零清除

**用法**:
```lisp
(m:trim realnum)
```

**参数**: 1 realnum  : 实数;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:trim (realnum / dimzin1 result)
    "数值后续零清除"
    (setq dimzin1 (getvar "dimzin"))
    (setvar "dimzin"
        8)
    (setq result (vl-catch-all-apply (quote rtos)
            (list realnum 2 8)))
    (setvar "dimzin"
        dimzin1)
    (if (not (vl-catch-all-error-p result))
        result))
```
</details>

---

### m:union

**说明**: 求列表的并集

**用法**:
```lisp
(m:union lst1 lst2)
```

**参数**: 1 lst1  : 列表;  2 lst2  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun m:union (lst1 lst2)
    "求列表的并集"
    (append lst1 (vl-remove-if (quote (lambda (x)
                    (member x lst1)))
            lst2)))
```
</details>

---

## matrix

**模块说明**: 矩阵运算、变换计算

### matrix:hadamard-product

**说明**: mxn矩阵与mxn 矩阵的Hadamard积记作A*B。其元素定义为两个矩阵对应元素的乘积的m×n矩阵

**用法**:
```lisp
(matrix:hadamard-product m1 m2)
```

**参数**: 1 m1  : 未识别定义;2 m2  : 未识别定义;

**返回值**: matrix

**示例**:
```lisp
(matrix:hadamard-product '((1 3 2)(1 0 0)(1 2 2)) '((0 0 2)(7 5 0)(2 1 1)));;=> ((0 0 4) (7 0 0) (2 2 2))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:hadamard-product (m1 m2)
  "mxn矩阵与mxn 矩阵的Hadamard积记作A*B。其元素定义为两个矩阵对应元素的乘积的m×n矩阵"
  "matrix"
  "(matrix:hadamard-product '((1 3 2)(1 0 0)(1 2 2))
    '((0 0 2)(7 5 0)(2 1 1)));;=> ((0 0 4)
    (7 0 0)
    (2 2 2))"
  (mapcar (quote (lambda (x y)
        (mapcar (quote *)
          x y)))
    m1 m2))
```
</details>

---

### matrix:kronecker-product

**说明**: 克罗内克积也称张量积、直积是两个任意大小的矩阵间的运算，符号记作(x) 。.以德国数学家利奥波德·克罗内克命名

**用法**:
```lisp
(matrix:kronecker-product m1 m2)
```

**参数**: 1 m1  : 未识别定义;2 m2  : 未识别定义;

**返回值**: matrix

**示例**:
```lisp
(matrix:kronecker-product '((1 2)(3 1)(5 3)) '((0 3)(2 1))) => ((0 3 0 6) (2 1 4 2) (0 9 0 3) (6 3 2 1) (0 15 0 9) (10 5 6 3))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:kronecker-product (m1 m2)
  "克罗内克积也称张量积、直积是两个任意大小的矩阵间的运算，符号记作(x)
  。.以德国数学家利奥波德·克罗内克命名"
  "matrix"
  "(matrix:kronecker-product '((1 2)(3 1)(5 3))
    '((0 3)(2 1)))
  => ((0 3 0 6)
    (2 1 4 2)
    (0 9 0 3)
    (6 3 2 1)
    (0 15 0 9)
    (10 5 6 3))"
  (apply (quote append)
    (mapcar (quote (lambda (a)
          (mapcar (quote (lambda (x)
                (apply (quote append)
                  x)))
            (matrix:trp a))))
      (mapcar (quote (lambda (m1%)
            (mapcar (quote (lambda (m1%%)
                  (mapcar (quote (lambda (m2%)
                        (mapcar (quote (lambda (m2%%)
                              (* m1%% m2%%)))
                          m2%)))
                    m2)))
              m1%)))
        m1))))
```
</details>

---

### matrix:mxm

**说明**: 矩阵相乘

**用法**:
```lisp
(matrix:mxm m q)
```

**参数**: 1 m  : 未识别定义;2 q  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:mxm (m q)
  "矩阵相乘"
  (mapcar (function (lambda (r)
        (matrix:mxv (matrix:trp q)
          r)))
    m))
```
</details>

---

### matrix:mxp

**说明**: 对点的坐标进行矩阵变换

**用法**:
```lisp
(matrix:mxp m p)
```

**参数**: 1 m  : 未识别定义;2 p  : 未识别定义;

**返回值**: 变换后的3D点坐标

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:mxp (m p)
  "对点的坐标进行矩阵变换"
  "变换后的3D点坐标"
  (if (= 2 (length p))
    (setq p (append p (list 0))))
  (reverse (cdr (reverse (matrix:mxv m (append p (list 1.0)))))))
```
</details>

---

### matrix:mxv

**说明**: 向量的矩阵变换(向量乘矩阵)

**用法**:
```lisp
(matrix:mxv m v)
```

**参数**: 1 m  : 未识别定义;2 v  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:mxv (m v)
  "向量的矩阵变换(向量乘矩阵)"
  (mapcar (function (lambda (r)
        (apply (quote +)
          (mapcar (quote *)
            r v))))
    m))
```
</details>

---

### matrix:norm

**说明**: 向量的模(长度)

**用法**:
```lisp
(matrix:norm v)
```

**参数**: 1 v  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:norm (v)
  "向量的模(长度)"
  (sqrt (apply (quote +)
      (mapcar (quote *)
        v v))))
```
</details>

---

### matrix:rotation

**说明**: 构造三维旋转矩阵，rx/ry/rz分别对应三个坐标轴的转角

**用法**:
```lisp
(matrix:rotation rx ry rz)
```

**参数**: 1 rx  : 未识别定义;2 ry  : 未识别定义;3 rz  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:rotation (rx ry rz)
  "构造三维旋转矩阵，rx/ry/rz分别对应三个坐标轴的转角"
  (matrix:mxm (matrix:rotation-x rx)
    (matrix:mxm (matrix:rotation-y ry)
      (matrix:rotation-z rz))))
```
</details>

---

### matrix:rotation-x

**说明**: 构造x轴旋转矩阵

**用法**:
```lisp
(matrix:rotation-x rx)
```

**参数**: 1 rx  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:rotation-x (rx / cr sr)
  "构造x轴旋转矩阵"
  (list (list 1 0 0 0)
    (list 0 (setq cr (cos rx))
      (setq sr (sin rx))
      0)
    (list 0 (- sr)
      cr 0)
    (list 0 0 0 1)))
```
</details>

---

### matrix:rotation-y

**说明**: 构造y轴旋转矩阵

**用法**:
```lisp
(matrix:rotation-y ry)
```

**参数**: 1 ry  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:rotation-y (ry / cr sr)
  "构造y轴旋转矩阵"
  (list (list (setq cr (cos ry))
      0 (- (setq sr (sin rz)))
      0)
    (list 0 1 0 0)
    (list sr 0 cr 0)
    (list 0 0 0 1)))
```
</details>

---

### matrix:rotation-z

**说明**: 构造z轴旋转矩阵

**用法**:
```lisp
(matrix:rotation-z rz)
```

**参数**: 1 rz  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:rotation-z (rz / crz srz)
  "构造z轴旋转矩阵"
  (list (list (setq crz (cos rz))
      (setq srz (sin rz))
      0 0)
    (list (- srz)
      crz 0 0)
    (list 0 0 1 0)
    (list 0 0 0 1)))
```
</details>

---

### matrix:scale

**说明**: 构造3轴缩放矩阵，xyz分别代表三轴的缩放比例

**用法**:
```lisp
(matrix:scale x y z)
```

**参数**: 1 x  : 未识别定义;2 y  : 未识别定义;3 z  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:scale (x y z)
  "构造3轴缩放矩阵，xyz分别代表三轴的缩放比例"
  (list (list x 0 0 0)
    (list 0 y 0 0)
    (list 0 0 z 0)
    (list 0 0 0 1)))
```
</details>

---

### matrix:scale1

**说明**: 构造3轴缩放矩阵，s代表三轴的统一缩放比例

**用法**:
```lisp
(matrix:scale1 s)
```

**参数**: 1 s  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:scale1 (s)
  "构造3轴缩放矩阵，s代表三轴的统一缩放比例"
  (list (list s 0 0 0)
    (list 0 s 0 0)
    (list 0 0 s 0)
    (list 0 0 0 1)))
```
</details>

---

### matrix:transform

**说明**: 坐标变换公式A'=TSRA

**用法**:
```lisp
(matrix:transform translation scale rotation pt)
```

**参数**: 1 translation  : 未识别定义;2 scale  : 比例值;3 rotation  : 未识别定义;4 pt  : 单个2D/3D坐标点;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:transform (translation scale rotation pt)
  "坐标变换公式A'=TSRA"
  (matrix:mxp translation (matrix:mxp scale (matrix:mxp rotation pt))))
```
</details>

---

### matrix:transformation

**说明**: 使用坐标变换公式A'=TSRA进行坐标变换

**用法**:
```lisp
(matrix:transformation translation scale rotation pt)
```

**参数**: 1 translation  : 未识别定义;2 scale  : 比例值;3 rotation  : 未识别定义;4 pt  : 单个2D/3D坐标点;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:transformation (translation scale rotation pt)
  "使用坐标变换公式A'=TSRA进行坐标变换"
  (matrix:mxp translation (matrix:mxp scale (matrix:mxp rotation pt))))
```
</details>

---

### matrix:translation

**说明**: 构造平移转换矩阵

**用法**:
```lisp
(matrix:translation x y z)
```

**参数**: 1 x  : 未识别定义;2 y  : 未识别定义;3 z  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:translation (x y z)
  "构造平移转换矩阵"
  (list (list 1 0 0 x)
    (list 0 1 0 y)
    (list 0 0 1 z)
    (list 0 0 0 1)))
```
</details>

---

### matrix:trp

**说明**: 矩阵转置

**用法**:
```lisp
(matrix:trp m)
```

**参数**: 1 m  : 未识别定义;

**返回值**: matrix

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:trp (m)
  "矩阵转置"
  "matrix"
  (apply (quote mapcar)
    (cons (quote list)
      m)))
```
</details>

---

### matrix:unit

**说明**: 单位向量

**用法**:
```lisp
(matrix:unit v)
```

**参数**: 1 v  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:unit (v)
  "单位向量"
  ((lambda (n)
      (if (equal 0.0 n 1.0e-14)
        nil (matrix:vxs v (/ 1.0 n))))
    (matrix:norm v)))
```
</details>

---

### matrix:v^v

**说明**: 两向量的叉积

**用法**:
```lisp
(matrix:v^v u v)
```

**参数**: 1 u  : 未识别定义;2 v  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:v^v (u v)
  "两向量的叉积"
  (list (- (* (cadr u)
        (caddr v))
      (* (cadr v)
        (caddr u)))
    (- (* (car v)
        (caddr u))
      (* (car u)
        (caddr v)))
    (- (* (car u)
        (cadr v))
      (* (car v)
        (cadr u)))))
```
</details>

---

### matrix:vxs

**说明**: 向量乘标量(系数)

**用法**:
```lisp
(matrix:vxs v s)
```

**参数**: 1 v  : 未识别定义;2 s  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:vxs (v s)
  "向量乘标量(系数)"
  (mapcar (quote (lambda (n)
        (* n s)))
    v))
```
</details>

---

### matrix:vxv

**说明**: 向量的点积

**用法**:
```lisp
(matrix:vxv v1 v2)
```

**参数**: 1 v1  : 未识别定义;2 v2  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun matrix:vxv (v1 v2)
  "向量的点积"
  (apply (quote +)
    (mapcar (quote *)
      v1 v2)))
```
</details>

---

## music-die

**模块说明**: 专用功能模块

### music-die:list-to-var

**用法**:
```lisp
(music-die:list-to-var lists)
```

**参数**: 1 lists  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun music-die:list-to-var (lists / pntlst2)
  (vlax-safearray-fill (vlax-make-safearray vlax-vbdouble (cons 0 (1- (* 3 (length lists)))))
    (foreach e lists (setq pntlst2 (append pntlst2 e)))))
```
</details>

---

### music-die:multi-element

**说明**: 使某个lisp元素出现以列表形式存储多份ELEMENT 元素  num 存储份数作者：MUSIC-DIE

**用法**:
```lisp
(music-die:multi-element element num)
```

**参数**: 1 element  : 未明确定义;2 num  : 未明确定义;

**返回值**: （list element elemen element element ...）

**示例**:
```lisp
(Multi-element 123 3) --> '(123 123 123)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun music-die:multi-element (element num / element-list)
  "\n使某个lisp元素出现以列表形式存储多份\nELEMENT 元素  num 存储份数\n作者：MUSIC-DIE\n"
  "\n返回值：（list element elemen element element ...）\n"
  "(Multi-element 123 3)
  --> '(123 123 123)"
  (repeat num (setq element-list (cons element element-list))))
```
</details>

---

### music-die:undo1

**说明**: 可撤回一步操作，并且不会使视口移动

**用法**:
```lisp
(music-die:undo1 )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun music-die:undo1 (/ ctr vsize)
  "可撤回一步操作，并且不会使视口移动"
  ""
  (setq ctr (trans (getvar "VIEWCTR")
      1 0))
  (setq vsize (getvar "VIEWSIZE"))
  (vl-cmdf "UNDO"
    1)
  (vla-zoomcenter *acad* (vlax-make-variant (music-die:list-to-var (list ctr)))
    vsize))
```
</details>

---

## orance

**模块说明**: 专用功能模块

### orance:show-all-objects

**说明**: 显示所有隐藏图元(属性为不可见)

**用法**:
```lisp
(orance:show-all-objects )
```

**参数**: None

**返回值**: 无返回值

**示例**:
```lisp
示例:(obj:Show-All-Objects)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun orance:show-all-objects nil "显示所有隐藏图元(属性为不可见)"
  "无返回值"
  "示例:(obj:Show-All-Objects)"
  (ssget "x"
    (quote ((60 . 1))))
  (progn (vlax-for obj (vla-get-activeselectionset (vla-get-activedocument (vlax-get-acad-object)))
      (vla-put-visible obj :vlax-true)))
  (princ))
```
</details>

---

## p

**模块说明**: 专用功能模块

### p:consp

**说明**: 判断是否为构造表，这里 nil 不是构造表

**用法**:
```lisp
(p:consp lst)
```

**参数**: 1 lst  : 列表;

**返回值**: T or nil

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:consp (lst)
  "判断是否为构造表，这里 nil 不是构造表"
  "T or nil"
  (vl-consp lst))
```
</details>

---

### p:curvep

**说明**: 是否是曲线

**用法**:
```lisp
(p:curvep obj)
```

**参数**: 1 obj  : activeX 对象;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:curvep (obj)
  "是否是曲线"
  (if (vlap obj)
    (and (member (vla-get-objectname obj)
        (quote ("AcDbPolyline"
            "AcDbSpline"
            "AcDb3dPolyline"
            "AcDb2dPolyline"
            "AcDbLine"
            "AcDbCircle"
            "AcDbArc"
            "AcDbEllipse"))))
    nil))
```
</details>

---

### p:dotpairp

**说明**: 是否为点对表

**用法**:
```lisp
(p:dotpairp lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:dotpairp (lst)
  "是否为点对表"
  (and (vl-consp lst)
    (not (vl-list-length lst))))
```
</details>

---

### p:ename-listp

**说明**: 判断是否为图元名列表

**用法**:
```lisp
(p:ename-listp lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:ename-listp (lst)
  "判断是否为图元名列表"
  (if (and lst (listp lst))
    (apply (quote and)
      (mapcar (quote p:enamep)
        lst))))
```
</details>

---

### p:enamep

**说明**: 判断是否图元

**用法**:
```lisp
(p:enamep arg)
```

**参数**: 1 arg  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:enamep (arg)
  "判断是否图元"
  (equal (type arg)
    (quote ename)))
```
</details>

---

### p:functionp

**说明**: 测试符号是不是函数,支持内部函数，用户函数，EXRX扩展函数和defun-q定义的函数

**用法**:
```lisp
(p:functionp sym)
```

**参数**: 1 sym  : 未识别定义;

**返回值**: T or nil

**示例**:
```lisp
(p:functionp boole)=> T
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:functionp (sym)
  "测试符号是不是函数,支持内部函数，用户函数，EXRX扩展函数和defun-q定义的函数"
  "T or nil"
  "(p:functionp boole)=> T"
  (or (member (type sym)
      (quote (subr usubr exrxsubr)))
    (and (not (null sym))
      (listp sym)
      (listp (car sym))
      (apply (quote and)
        (mapcar (quote vl-symbolp)
          (caar sym))))))
```
</details>

---

### p:intp

**说明**: 判断是否整数

**用法**:
```lisp
(p:intp x)
```

**参数**: 1 x  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:intp (x)
  "判断是否整数"
  (equal (type x)
    (quote int)))
```
</details>

---

### p:listp

**说明**: 判断是否为链表。注意'(a b . c)不是链表

**用法**:
```lisp
(p:listp lst)
```

**参数**: 1 lst  : 列表;

**返回值**: T or nil

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:listp (lst)
  "判断是否为链表。注意'(a b . c)不是链表"
  "T or nil"
  (and (listp lst)
    (vl-list-length lst)))
```
</details>

---

### p:matrixp

**说明**: 测试一组列表是否为矩阵

**用法**:
```lisp
(p:matrixp mat)
```

**参数**: 1 mat  : 未识别定义;

**返回值**: T or nil

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:matrixp (mat)
  "测试一组列表是否为矩阵"
  "T or nil"
  (and (apply (quote and)
      (mapcar (quote listp)
        mat))
    (apply (quote =)
      (mapcar (quote length)
        mat))))
```
</details>

---

### p:phyunitp

**说明**: 测试字符串是否为物理量单位SI

**用法**:
```lisp
(p:phyunitp str)
```

**参数**: 1 str  : 字符串;

**返回值**: t or nil

**示例**:
```lisp
(p:phyunitp "W/mU+00B2·K")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:phyunitp (str)
  "测试字符串是否为物理量单位SI"
  "t or nil"
  "(p:phyunitp \"W/m\U+00B2·K\")"
  (or (member str @:*units*)
    (apply (quote and)
      (mapcar (quote (lambda (x)
            (member (chr x)
              @:*units*)))
        (string:s2l-ansi str)))))
```
</details>

---

### p:picksetp

**说明**: 判断是否非空选择集

**用法**:
```lisp
(p:picksetp ss)
```

**参数**: 1 ss  : 选择集;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:picksetp (ss)
  "判断是否非空选择集"
  (and (= (type ss)
      (quote pickset))
    (> (sslength ss)
      0)))
```
</details>

---

### p:readme

**说明**: 谓词函数及测试函数。

**用法**:
```lisp
(p:readme )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:readme nil "谓词函数及测试函数。"
  (princ "谓词函数及测试函数。使用 (require 'p:*)
    加载这些函数")
  (princ))
```
</details>

---

### p:realp

**说明**: 判断是否实数

**用法**:
```lisp
(p:realp arg)
```

**参数**: 1 arg  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:realp (arg)
  "判断是否实数"
  (equal (type arg)
    (quote real)))
```
</details>

---

### p:safearrayp

**说明**: 判断是否为安全数组

**用法**:
```lisp
(p:safearrayp x)
```

**参数**: 1 x  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:safearrayp (x)
  "判断是否为安全数组"
  (equal (type x)
    (quote safearray)))
```
</details>

---

### p:string-listp

**说明**: 判断是否为字符串列表

**用法**:
```lisp
(p:string-listp lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:string-listp (lst)
  "判断是否为字符串列表"
  (if (and lst (listp lst))
    (apply (quote and)
      (mapcar (quote p:stringp)
        lst))))
```
</details>

---

### p:stringp

**说明**: 判断是否字符串

**用法**:
```lisp
(p:stringp arg)
```

**参数**: 1 arg  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:stringp (arg)
  "判断是否字符串"
  (equal (type arg)
    (quote str)))
```
</details>

---

### p:variantp

**说明**: 判断是否变体

**用法**:
```lisp
(p:variantp arg)
```

**参数**: 1 arg  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:variantp (arg)
  "判断是否变体"
  (equal (type arg)
    (quote variant)))
```
</details>

---

### p:vla-listp

**说明**: 判断是否为vla对象列表

**用法**:
```lisp
(p:vla-listp lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:vla-listp (lst)
  "判断是否为vla对象列表"
  (if (and lst (listp lst))
    (apply (quote and)
      (mapcar (quote p:vlap)
        lst))))
```
</details>

---

### p:vlap

**说明**: 判断是否vla对象.

**用法**:
```lisp
(p:vlap obj)
```

**参数**: 1 obj  : activeX 对象;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun p:vlap (obj)
  "判断是否vla对象."
  (equal (type obj)
    (quote vla-object)))
```
</details>

---

## pickset

**模块说明**: 选择集处理、过滤、转换

### pickset:boxs

**说明**: 求选择集内各图元的包围盒。(角点:左下，右上)组成的列表,ss 选择集或图元列表

**用法**:
```lisp
(pickset:boxs ss)
```

**参数**: 1 ss  : 选择集;

**返回值**: (box1 box2 box3)

**示例**:
```lisp
(pickset:boxs (ssget))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:boxs (ss / n en lst)
  "求选择集内各图元的包围盒。(角点:左下，右上)组成的列表,ss 选择集或图元列表"
  "(box1 box2 box3)"
  "(pickset:boxs (ssget))"
  (if (p:picksetp ss)
    (setq ss (pickset:to-list ss)))
  (if (p:ename-listp ss)
    (mapcar (quote (lambda (x)
          (entity:getbox x 0)))
      ss)))
```
</details>

---

### pickset:cluster

**说明**: 对图元进行聚类分析，按片区求图元集的包围盒。

**用法**:
```lisp
(pickset:cluster ss gap)
```

**参数**: 1 ss  : 选择集;2 gap  : 未明确定义;

**返回值**: 各图元群的包围盒(两点坐标), 组成的列表

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:cluster (ss gap / a b c ca cc flag l l1 lst n)
  "对图元进行聚类分析，按片区求图元集的包围盒。"
  "各图元群的包围盒(两点坐标), 组成的列表"
  (if ss (progn (setq l (vl-sort (pickset:boxs ss)
          (quote (lambda (box1 box2 / ax1 ax2 ay1 bx1 bx2 by1)
              (if (equal (setq ax1 (caar box1))
                  (setq bx1 (caar box2))
                  0.001)
                (if (equal (setq ay1 (cadar box1))
                    (setq by1 (cadar box2))
                    0.001)
                  (if (equal (setq ax2 (caadr box1))
                      (setq bx2 (caadr box2))
                      0.001)
                    (< (cadadr box1)
                      (cadadr box2))
                    (< ax2 bx2))
                  (< ay1 by1))
                (< ax1 bx1))))))
      (setq gap (* gap 0.5))
      (setq l (mapcar (quote (lambda (x)
              (list (list (- (caar x)
                    gap)
                  (- (cadar x)
                    gap))
                (list (+ (caadr x)
                    gap)
                  (+ (cadadr x)
                    gap)))))
          l))
      (setq k t r nil)
      (while k (setq a (car l)
          lst nil)
        (while (setq ca (car l))
          (setq l (cdr l))
          (if (geometry:box-intersectp a (setq ca (car l)))
            (progn (setq c (geometry:merge-box a ca))
              (setq a c))
            (if (setq cc (vl-some (quote (lambda (x)
                      (if (geometry:box-intersectp a x)
                        (list (geometry:merge-box a x)
                          x))))
                  lst))
              (progn (if (not (equal (car cc)
                      (cadr cc)))
                  (setq lst (subst (car cc)
                      (cadr cc)
                      lst)))
                (setq a ca))
              (setq lst (cons a lst)
                a ca))))
        (if (= (length lst)
            (length r))
          (setq k nil)
          (setq r lst l lst)))))
  (setq l (mapcar (quote (lambda (x)
          (list (list (- (caar x)
                gap)
              (- (cadar x)
                gap))
            (list (+ (caadr x)
                gap)
              (+ (cadadr x)
                gap)))))
      lst)))
```
</details>

---

### pickset:delsameent

**说明**: 删除重复图元参数:ss:选择集

**用法**:
```lisp
(pickset:delsameent ss)
```

**参数**: 1 ss  : 选择集;

**返回值**: 无

**示例**:
```lisp
(entity:DelSameEnt (ssget))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:delsameent (ss / list1 s9 xy)
  "删除重复图元\n参数:\nss:选择集"
  "无"
  "(entity:DelSameEnt (ssget))"
  (foreach e (pickset->list ss)
    (setq xy (cdr (entget e)))
    (if (setq s9 (assoc 5 xy))
      (setq xy (subst (quote (5 . "ASD"))
          s9 xy)))
    (if (member xy list1)
      (entdel e)
      (setq list1 (cons xy list1))))
  (princ))
```
</details>

---

### pickset:erase

**说明**: 删除选择集图元

**用法**:
```lisp
(pickset:erase ss)
```

**参数**: 1 ss  : 选择集;

**返回值**: 最后一个被删除的图元

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:erase (ss / e)
  "删除选择集图元"
  "最后一个被删除的图元"
  (while (> (sslength ss)
      0)
    (setq e (ssname ss 0))
    (setq ss (ssdel e ss))
    (entdel e)))
```
</details>

---

### pickset:from-entlist

**说明**: 图元列表->选择集

**用法**:
```lisp
(pickset:from-entlist entlst)
```

**参数**: 1 entlst  : 单个图元;

**返回值**: 选择集

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:from-entlist (entlst / ss)
  "图元列表->选择集"
  "选择集"
  (setq ss (ssadd))
  (foreach i entlst (ssadd i ss))
  ss)
```
</details>

---

### pickset:from-list

**说明**: 把图元列表转化成选择集，参数

**用法**:
```lisp
(pickset:from-list lstent)
```

**参数**: 1 lstent  : 列表;

**返回值**: 选择集

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:from-list (lstent / ss)
  "把图元列表转化成选择集，参数"
  "选择集"
  (setq ss (ssadd))
  (foreach i lstent (ssadd i ss))
  ss)
```
</details>

---

### pickset:get-sub

**说明**: 从选择集或图元表中按 filter 规则过滤. 当前版本不支持 XOR 和 逻辑嵌套。

**用法**:
```lisp
(pickset:get-sub ss filter)
```

**参数**: 1 ss  : 选择集;2 filter  : 过滤dxf组码;

**返回值**: 过滤后的图元表

**示例**:
```lisp
(pickset:get-sub ss '((-4 . "")(1 . "7")(-4 . "<")(1 . "4")(-4 . "OR>")))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:get-sub (ss filter / stack-logic state-compare ss-mid to-filter compares)
  "从选择集或图元表中按 filter 规则过滤. 当前版本不支持 XOR 和 逻辑嵌套。"
  "过滤后的图元表"
  "(pickset:get-sub ss '((-4 . \"<OR\")(1 . \"1*\")(-4 . \">\")(1 . \"7\")(-4 . \"<\")(1 . \"4\")(-4 . \"OR>\")))"
  (if (= (quote pickset)
      (type ss))
    (setq ss (pickset:to-list ss)))
  (setq stack-logic (quote nil))
  (setq state-compare nil)
  (setq ss-mid (quote nil))
  (defun compares (x)
    (if (atom (cdr (assoc (car filter)
            (entget x))))
      ((eval (read state-compare))
        (cdr (assoc (car filter)
            (entget x)))
        (cdr filter))
      (apply (quote and)
        (mapcar (quote (lambda (op m n)
              ((eval (read op))
                m n)))
          (string:to-list state-compare ",")
          (cdr (assoc (car filter)
              (entget x)))
          (cdr filter)))))
  (defun to-filter (filter / ss-n op)
    (cond ((null (car stack-logic))
        (cond ((null state-compare)
            (if (= (quote str)
                (type (cdr filter)))
              (setq ss (vl-remove-if-not (quote (lambda (x)
                      (and (= (quote str)
                          (type (cdr (assoc (car filter)
                                (entget x)))))
                        (wcmatch (strcase (cdr (assoc (car filter)
                                (entget x))))
                          (strcase (cdr filter))))))
                  ss))
              (setq ss (vl-remove-if-not (quote (lambda (x)
                      (member filter (entget x))))
                  ss))))
          (t (setq != /=)
            (setq <> /=)
            (setq ss (vl-remove-if-not (quote (lambda (x)
                    (compares x)))
                ss)))))
      ((= "<AND"
          (strcase (car stack-logic)))
        (cond ((null state-compare)
            (if (= (quote str)
                (type (cdr filter)))
              (setq ss (vl-remove-if-not (quote (lambda (x)
                      (and (= (quote str)
                          (type (cdr (assoc (car filter)
                                (entget x)))))
                        (wcmatch (strcase (cdr (assoc (car filter)
                                (entget x))))
                          (strcase (cdr filter))))))
                  ss))
              (setq ss (vl-remove-if-not (quote (lambda (x)
                      (member filter (entget x))))
                  ss))))
          (t (setq != /=)
            (setq <> /=)
            (setq ss (append ss (vl-remove-if-not (quote (lambda (x)
                      (compares x)))
                  ss))))))
      ((= "<NOT"
          (strcase (car stack-logic)))
        (cond ((null state-compare)
            (if (= (quote str)
                (type (cdr filter)))
              (setq ss (vl-remove-if (quote (lambda (x)
                      (and (= (quote str)
                          (type (cdr (assoc (car filter)
                                (entget x)))))
                        (wcmatch (strcase (cdr (assoc (car filter)
                                (entget x))))
                          (strcase (cdr filter))))))
                  ss))
              (setq ss (vl-remove-if (quote (lambda (x)
                      (member filter (entget x))))
                  ss))))
          (t (setq != /=)
            (setq <> /=)
            (setq ss (append ss (vl-remove-if (quote (lambda (x)
                      (compares x)))
                  ss))))))
      ((= "<OR"
          (strcase (car stack-logic)))
        (cond ((null state-compare)
            (if (= (quote str)
                (type (cdr filter)))
              (setq ss-mid (append ss-mid (vl-remove-if-not (quote (lambda (x)
                        (and (= (quote str)
                            (type (cdr (assoc (car filter)
                                  (entget x)))))
                          (wcmatch (strcase (cdr (assoc (car filter)
                                  (entget x))))
                            (strcase (cdr filter))))))
                    ss)))
              (setq ss-mid (append ss-mid (vl-remove-if-not (quote (lambda (x)
                        (member filter (entget x))))
                    ss)))))
          (t (setq != /=)
            (setq <> /=)
            (setq ss-mid (append ss-mid (vl-remove-if-not (quote (lambda (x)
                      (compares x)))
                  ss))))))
      (t nil)))
  (foreach filter% filter (cond ((= (car filter%)
          -4)
        (cond ((member (read (cdr filter%))
              (mapcar (quote read)
                (quote ("<AND"
                    "<NOT"
                    "<OR"
                    "<XOR"))))
            (setq stack-logic (cons (cdr filter%)
                stack-logic)))
          ((member (read (cdr filter%))
              (mapcar (quote read)
                (quote ("AND>"
                    "NOT>"))))
            (setq state-compare nil)
            (setq stack-logic (cdr stack-logic)))
          ((member (read (cdr filter%))
              (mapcar (quote read)
                (quote ("OR>"
                    "XOR>"))))
            (setq state-compare nil)
            (setq ss ss-mid)
            (setq ss-mid (quote nil))
            (setq stack-logic (cdr stack-logic)))
          (t (setq state-compare (cdr filter%)))))
      (t (to-filter filter%))))
  (if ss-mid (setq ss ss-mid))
  ss)
```
</details>

---

### pickset:getbox

**说明**: 获取选择集的包围盒。ss 为选择集，图元或图元列表

**用法**:
```lisp
(pickset:getbox ss offset)
```

**参数**: 1 ss  : 选择集;2 offset  : 偏移量;

**返回值**: 外框（偏移后）的左下，右上角点

**示例**:
```lisp
(pickset:getbox sel 0.2)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:getbox (ss offset / ptlist)
  "获取选择集的包围盒。ss 为选择集，图元或图元列表"
  "外框（偏移后）的左下，右上角点"
  "(pickset:getbox sel 0.2)"
  (cond ((= (quote pickset)
        (type ss))
      (setq ss (pickset:to-list ss)))
    ((= (quote ename)
        (type ss))
      (setq ss (list ss)))
    ((p:vla-listp ss)
      (setq ss (mapcar (quote o2e)
          ss))))
  (if (p:ename-listp ss)
    (progn (setq ptlist (apply (quote append)
          (mapcar (quote (lambda (x)
                (entity:getbox x offset)))
            ss)))
      (list (apply (quote mapcar)
          (cons (quote min)
            ptlist))
        (apply (quote mapcar)
          (cons (quote max)
            ptlist))))
    (progn (@:log "ERROR"
        "parameter is NOT pickset")
      nil)))
```
</details>

---

### pickset:intersect

**说明**: 求两个选择集的交集，ss1,ss2 为选择集，图元列表。

**用法**:
```lisp
(pickset:intersect ss1 ss2)
```

**参数**: 1 ss1  : 选择集;2 ss2  : 选择集;

**返回值**: 图元列表

**示例**:
```lisp
(pickset:intersect (ssget)(ssget))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:intersect (ss1 ss2)
  "求两个选择集的交集，ss1,ss2 为选择集，图元列表。"
  "图元列表"
  "(pickset:intersect (ssget)(ssget))"
  (if (p:picksetp ss1)
    (setq ss1 (pickset:to-list ss1)))
  (if (p:picksetp ss2)
    (setq ss2 (pickset:to-list ss2)))
  (if (and (p:ename-listp ss1)
      (p:ename-listp ss2))
    (vl-remove-if (quote (lambda (x)
          (not (member x ss2))))
      ss1)))
```
</details>

---

### pickset:join

**说明**: 将第一个选择集中的图元加入到第二个选择集中。

**用法**:
```lisp
(pickset:join ss1 ss2)
```

**参数**: 1 ss1  : 选择集;2 ss2  : 选择集;

**返回值**: 合并后的新选择集

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:join (ss1 ss2 / i n ename)
  "将第一个选择集中的图元加入到第二个选择集中。"
  "合并后的新选择集"
  (setq i -1)
  (setq n (sslength ss1))
  (while (< (setq i (1+ i))
      n)
    (setq ss2 (ssadd (ssname ss1 i)
        ss2)))
  ss2)
```
</details>

---

### pickset:length

**说明**: 返回选择集或图元列表内图元的个数。非选择集和图元列表，则返回nil

**用法**:
```lisp
(pickset:length ss)
```

**参数**: 1 ss  : 选择集;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:length (ss)
  "返回选择集或图元列表内图元的个数。非选择集和图元列表，则返回nil"
  (cond ((= (quote pickset)
        (type ss))
      (sslength ss))
    ((and (listp ss)
        (p:ename-listp ss))
      (length ss))))
```
</details>

---

### pickset:pt-verts

**说明**: 取点

**用法**:
```lisp
(pickset:pt-verts ss)
```

**参数**: 1 ss  : 选择集;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:pt-verts (ss / i listpp a b c d)
  "取点"
  (setq i 0 listpp nil)
  (if ss (repeat (sslength ss)
      (setq a (ssname ss i))
      (setq b (entget a))
      (setq ename (cdr (assoc 0 b)))
      (cond ((or nil (= ename "POLYLINE")
            (= ename "LWPOLYLINE"))
          (progn (setq c (getlistofpline a))
            (setq listpp (append c listpp))))
        ((= ename "LINE")
          (progn (setq c (cdr (assoc 10 b)))
            (setq d (cdr (assoc 11 b)))
            (setq listpp (cons c listpp))
            (setq listpp (cons d listpp))))
        ((= ename "POINT")
          (progn (setq c (cdr (assoc 10 b)))
            (setq listpp (cons c listpp)))))
      (setq i (1+ i))))
  listpp)
```
</details>

---

### pickset:ptx

**说明**: 取选择集4角点坐标的第n个，左下 0 右下 1 右上 2 左上 3

**用法**:
```lisp
(pickset:ptx sel n)
```

**参数**: 1 sel  : 选择集;2 n  : 未明确定义;

**返回值**: 第n个角点坐标

**示例**:
```lisp
(pickset:ptx sel 0)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:ptx (sel n / ptlist)
  "取选择集4角点坐标的第n个，左下 0 右下 1 右上 2 左上 3"
  "第n个角点坐标"
  "(pickset:ptx sel 0)"
  (setq ptlist (pickset:getbox sel 0))
  (nth n (point:rec-2pt->4pt (car ptlist)
      (cadr ptlist))))
```
</details>

---

### pickset:readme

**说明**: 选择集相关函数。

**用法**:
```lisp
(pickset:readme )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:readme nil "选择集相关函数。"
  (princ "选择集相关函数。使用 (require 'pickset:*)
    加载这些函数")
  (princ))
```
</details>

---

### pickset:sort

**说明**: 图元列表按组码10的坐标进行排序。参数:ssPts:图元列表;参数:KEY:xyzXYZ 任意组合 ,例如"yX",y在前表示y坐标优先，小y表示从小到大(注:二维点时，不能有z);参数:Fuzz:数值或列表，允许偏差；

**用法**:
```lisp
(pickset:sort sspts key fuzz)
```

**参数**: 1 sspts  : 选择集;2 key  : 键，关键字;3 fuzz  : 容差;

**返回值**: 图元列表

**示例**:
```lisp
(pickset:sort (ssget) "Yx" '(3000 10)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:sort (sspts key fuzz / keys funs orders equaled)
  "图元列表按组码10的坐标进行排序。参数:ssPts:图元列表;参数:KEY:xyzXYZ 任意组合 ,例如\"yX\",y在前表示y坐标优先，小y表示从小到大(注:二维点时，不能有z);参数:Fuzz:数值或列表，允许偏差；"
  "图元列表"
  "(pickset:sort (ssget)
    \"Yx\"
    '(3000 10)"
    (setq keys (vl-string->list key))
    (setq funs (mapcar (quote (lambda (xyz)
            (if (< xyz 100)
              > <)))
        keys))
    (setq orders (mapcar (quote (lambda (xyz)
            (cond ((< xyz 100)
                (- xyz 88))
              (t (- xyz 120)))))
        keys))
    (defun equal-compare (fun a b fuzz)
      (if equaled (if (not (equal a b fuzz))
          (progn (setq equaled nil)
            (fun a b)))))
    (if (atom fuzz)
      (setq fuzz (list fuzz)))
    (while (< (length fuzz)
        (length keys))
      (setq fuzz (append fuzz (list (last fuzz)))))
    (if (p:picksetp sspts)
      (setq sspts (pickset:to-list sspts)))
    (vl-sort sspts (quote (lambda (x y / i equaled)
          (setq equaled t)
          (apply (quote or)
            (mapcar (quote equal-compare)
              funs (mapcar (quote (lambda (m)
                    (nth m (entity:getdxf x 10))))
                orders)
              (mapcar (quote (lambda (m)
                    (nth m (entity:getdxf y 10))))
                orders)
              fuzz))))))
```
</details>

---

### pickset:sort-by-box

**说明**: 图元列表按包围盒的中心点的坐标进行排序。参数:ssPts:图元列表;参数:KEY:xyzXYZ 任意组合 ,例如"yX",y在前表示y坐标优先，小y表示从小到大(注:二维点时，不能有z);参数:Fuzz:数值或列表，允许偏差；

**用法**:
```lisp
(pickset:sort-by-box sspts key fuzz)
```

**参数**: 1 sspts  : 选择集;2 key  : 键，关键字;3 fuzz  : 容差;

**返回值**: 图元列表

**示例**:
```lisp
(pickset:sort1 (ssget) "Yx" '(3000 10)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:sort-by-box (sspts key fuzz / keys funs orders equaled)
  "图元列表按包围盒的中心点的坐标进行排序。参数:ssPts:图元列表;参数:KEY:xyzXYZ 任意组合 ,例如\"yX\",y在前表示y坐标优先，小y表示从小到大(注:二维点时，不能有z);参数:Fuzz:数值或列表，允许偏差；"
  "图元列表"
  "(pickset:sort1 (ssget)
    \"Yx\"
    '(3000 10)"
    (setq keys (vl-string->list key))
    (setq funs (mapcar (quote (lambda (xyz)
            (if (< xyz 100)
              > <)))
        keys))
    (setq orders (mapcar (quote (lambda (xyz)
            (cond ((< xyz 100)
                (- xyz 88))
              (t (- xyz 120)))))
        keys))
    (defun equal-compare (fun a b fuzz)
      (if equaled (if (not (equal a b fuzz))
          (progn (setq equaled nil)
            (fun a b)))))
    (if (atom fuzz)
      (setq fuzz (list fuzz)))
    (while (< (length fuzz)
        (length keys))
      (setq fuzz (append fuzz (list (last fuzz)))))
    (if (p:picksetp sspts)
      (setq sspts (pickset:to-list sspts)))
    (vl-sort sspts (quote (lambda (x y / i equaled)
          (setq equaled t)
          (apply (quote or)
            (mapcar (quote equal-compare)
              funs (mapcar (quote (lambda (m)
                    (nth m (point:centroid (entity:getbox x 0)))))
                orders)
              (mapcar (quote (lambda (m)
                    (nth m (point:centroid (entity:getbox y 0)))))
                orders)
              fuzz))))))
```
</details>

---

### pickset:sort-with-dxf

**说明**: 选择集按照给定的组码值进行排序。n:如果组码值为一个表，则 n 指出使用第几个；否则nil 。 decreasep:T表示从大到小，nil表示从小到大

**用法**:
```lisp
(pickset:sort-with-dxf ss dxf-i n fuzz decreasep)
```

**参数**: 1 ss  : 选择集;2 dxf-i  : 组码号;3 n  : 未识别定义;4 fuzz  : 容差;5 decreasep  : 未识别定义;

**返回值**: 排序后的选择集

**示例**:
```lisp
(pickset:sort-with-dxf SS 62 nil 0 T) ;;表示按照62(颜色号)组码的值进行排序，顺序为从大到小
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:sort-with-dxf (ss dxf-i n fuzz decreasep / ent index lst newlst newse tmp)
  "选择集按照给定的组码值进行排序。<br />n:如果组码值为一个表，则 n 指出使用第几个；否则nil 。<br /> decreasep:T表示从大到小，nil表示从小到大 "
  "排序后的选择集"
  "(pickset:sort-with-dxf SS 62 nil 0 T)
  ;;表示按照62(颜色号)组码的值进行排序，顺序为从大到小"
  (setq lst (quote nil)
    index 0)
  (repeat (sslength ss)
    (setq ent (entget (ssname ss index))
      tmp (cdr (assoc dxf-i ent)))
    (if (and n (= (type n)
          (quote int))
        (= (type tmp)
          (quote list))
        (< n (length tmp)))
      (setq tmp (nth n tmp)))
    (setq lst (cons (list tmp (cdr (assoc 5 ent)))
        lst))
    (setq index (1+ index)))
  (if (and fuzz (or (= (type fuzz)
          (quote int))
        (= (type fuzz)
          (quote real)))
      (or (= (type tmp)
          (quote int))
        (= (type tmp)
          (quote real))))
    (setq newlst (vl-sort lst (function (lambda (e1 e2)
            (< (+ (car e1)
                fuzz)
              (car e2))))))
    (setq newlst (vl-sort lst (function (lambda (e1 e2)
            (< (car e1)
              (car e2)))))))
  (if decreasep (setq newlst (reverse newlst)))
  (setq newse (ssadd))
  (foreach tmp newlst (setq newse (ssadd (handent (cadr tmp))
        newse)))
  newse)
```
</details>

---

### pickset:sort1

**说明**: 测试版，图元列表按组码10的坐标进行排序。参数:ssPts:图元列表;参数:KEY:xyzXYZ 任意组合 ,例如"yX",y在前表示y坐标优先，小y表示从小到大(注:二维点时，不能有z);参数:Fuzz:数值或列表，允许偏差；

**用法**:
```lisp
(pickset:sort1 sspts key fuzz)
```

**参数**: 1 sspts  : 选择集;2 key  : 键，关键字;3 fuzz  : 容差;

**返回值**: 图元列表

**示例**:
```lisp
(pickset:sort1 (ssget) "Yx" '(3000 10)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:sort1 (sspts key fuzz / keys funs orders equaled)
  "测试版，图元列表按组码10的坐标进行排序。参数:ssPts:图元列表;参数:KEY:xyzXYZ 任意组合 ,例如\"yX\",y在前表示y坐标优先，小y表示从小到大(注:二维点时，不能有z);参数:Fuzz:数值或列表，允许偏差；"
  "图元列表"
  "(pickset:sort1 (ssget)
    \"Yx\"
    '(3000 10)"
    (setq keys (vl-string->list key))
    (setq funs (mapcar (quote (lambda (xyz)
            (if (< xyz 100)
              > <)))
        keys))
    (setq orders (mapcar (quote (lambda (xyz)
            (cond ((< xyz 100)
                (- xyz 88))
              (t (- xyz 120)))))
        keys))
    (defun equal-compare (fun a b fuzz)
      (if equaled (if (not (equal a b fuzz))
          (progn (setq equaled nil)
            (fun a b)))))
    (if (atom fuzz)
      (setq fuzz (list fuzz)))
    (while (< (length fuzz)
        (length keys))
      (setq fuzz (append fuzz (list (last fuzz)))))
    (if (p:picksetp sspts)
      (setq sspts (pickset:to-list sspts)))
    (vl-sort sspts (quote (lambda (x y / i equaled)
          (setq equaled t)
          (apply (quote or)
            (mapcar (quote equal-compare)
              funs (mapcar (quote (lambda (m)
                    (nth m (entity:getdxf x 10))))
                orders)
              (mapcar (quote (lambda (m)
                    (nth m (entity:getdxf y 10))))
                orders)
              fuzz))))))
```
</details>

---

### pickset:ss-forword-en

**说明**: 将图元 ent 之后的所有图元形成的选择集

**用法**:
```lisp
(pickset:ss-forword-en en)
```

**参数**: 1 en  : 单个图元;

**返回值**: pickset

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:ss-forword-en (en / ss)
  "将图元 ent 之后的所有图元形成的选择集"
  "pickset"
  (if en (progn (setq ss (ssadd))
      (while (setq en (entnext en))
        (if (not (member (cdr (assoc 0 (entget en)))
              (quote ("ATTRIB"
                  "VERTEX"
                  "SEQEND"))))
          (ssadd en ss)))
      (if (zerop (sslength ss))
        (setq ss nil))
      ss)
    (ssget "_x")))
```
</details>

---

### pickset:ssget

**说明**: 自定义带提示符的ssget

**用法**:
```lisp
(pickset:ssget msg params)
```

**参数**: 1 msg  : 提示信息;2 params  : 参数列表;

**返回值**: 选择集

**示例**:
```lisp
(pickset:ssget "选择对象：" '("_WP" pt_list ((0 . "LINE") (62 . 5))))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:ssget (msg params / sel)
  "自定义带提示符的ssget "
  "选择集"
  "(pickset:ssget \"选择对象：\"
    '(\"_WP\"
      pt_list ((0 . \"LINE\")
        (62 . 5))))"
  (princ msg)
  (setvar (quote nomutt)
    1)
  (setq sel (vl-catch-all-apply (quote ssget)
      params))
  (setvar (quote nomutt)
    0)
  (if (not (vl-catch-all-error-p sel))
    sel))
```
</details>

---

### pickset:ssget-crossline

**说明**: 取得与线相交的选择集

**用法**:
```lisp
(pickset:ssget-crossline ent filter)
```

**参数**: 1 ent  : 单个图元;2 filter  : 过滤dxf组码;

**返回值**: 选择集

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:ssget-crossline (ent filter /)
  "取得与线相交的选择集"
  "选择集"
  (if filter (ssget "f"
      (entity:getdxf ent (quote (10 11)))
      filter)
    (ssget "f"
      (entity:getdxf ent (quote (10 11))))))
```
</details>

---

### pickset:sub

**说明**: 选择集相减

**用法**:
```lisp
(pickset:sub ss1 ss2)
```

**参数**: 1 ss1  : 选择集;2 ss2  : 选择集;

**返回值**: 选择集 or nil

**示例**:
```lisp
(pickset:Sub (setq ss1 (ssget)) (setq ss2 (ssget)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:sub (ss1 ss2 / ename ss sstemp)
  "选择集相减"
  "选择集 or nil"
  "(pickset:Sub (setq ss1 (ssget))
    (setq ss2 (ssget)))"
  (cond ((and (equal (type ss1)
          (quote pickset))
        (equal (type ss2)
          (quote pickset)))
      (cond ((equal (sslength ss1)
            (sslength ss2))
          (vl-cmdf "_.select"
            ss1 "")
          (setq ss (ssget "p"))
          (vl-cmdf "_.select"
            ss2 "")
          (setq sstemp (ssget "p"))
          (repeat (sslength sstemp)
            (setq ename (ssname sstemp 0))
            (ssdel ename sstemp)
            (if (ssmemb ename ss)
              (ssdel ename ss)))
          (if (equal (sslength ss)
              0)
            nil ss))
        (t (command "._Select"
            ss1 "_Remove"
            ss2 "")
          (ssget "_P"))))
    ((and (equal (type ss1)
          (quote pickset))
        (not (equal (type ss2)
            (quote pickset))))
      ss1)
    (t nil)))
```
</details>

---

### pickset:to-array

**说明**: 选择集->数

**用法**:
```lisp
(pickset:to-array ss)
```

**参数**: 1 ss  : 选择集;

**返回值**: 数组

**示例**:
```lisp
(pickset->Array (ssget))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:to-array (ss)
  "选择集->数"
  "数组"
  "(pickset->Array (ssget))"
  (if ss (vla:list->array (pickset:to-vlalist ss)
      9)
    nil))
```
</details>

---

### pickset:to-ax

**说明**: 将选择集或图元列表转化为ActiveX选择集对象

**用法**:
```lisp
(pickset:to-ax ss)
```

**参数**: 1 ss  : 选择集;

**返回值**: Object

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:to-ax (ss / ssetobj objs cnt)
  "将选择集或图元列表转化为ActiveX选择集对象"
  "Object"
  (if (= (quote pickset)
      (type ss))
    (setq ss (pickset:to-list ss)))
  (setq ssetobj (vla-add (vla-get-selectionsets *doc*)
      (strcat "ss"
        (@:timestamp))))
  (setq objs (vlax-make-safearray vlax-vbobject (cons 0 (1- (length ss))))
    cnt 0)
  (foreach obj% (pickset:to-vlalist ss)
    (vlax-safearray-put-element objs cnt obj%)
    (setq cnt (1+ cnt)))
  (vla-additems ssetobj objs)
  ssetobj)
```
</details>

---

### pickset:to-entlist

**说明**: 选择集->图元列表

**用法**:
```lisp
(pickset:to-entlist ss)
```

**参数**: 1 ss  : 选择集;

**返回值**: 图元列表

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:to-entlist (ss)
  "选择集->图元列表"
  "图元列表"
  (if ss (pickset:to-list-by-ssnamex ss)))
```
</details>

---

### pickset:to-list

**说明**: 选择集转图元列表

**用法**:
```lisp
(pickset:to-list ss)
```

**参数**: 1 ss  : 选择集;

**返回值**: 图元列表

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:to-list (ss)
  "选择集转图元列表"
  "图元列表"
  (if ss (pickset:to-list-by-ssnamex ss)))
```
</details>

---

### pickset:to-list-by-ssname

**说明**: 选择集转图元表，ssname 方法

**用法**:
```lisp
(pickset:to-list-by-ssname ss)
```

**参数**: 1 ss  : 选择集;

**返回值**: lst

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:to-list-by-ssname (ss / i lst)
  "选择集转图元表，ssname 方法"
  "lst"
  ""
  (setq i -1)
  (if (p:picksetp ss)
    (repeat (sslength ss)
      (setq lst (cons (ssname ss (setq i (1+ i)))
          lst))))
  (reverse lst))
```
</details>

---

### pickset:to-list-by-ssnamex

**说明**: 选择集转图元表，ssnamex 方法

**用法**:
```lisp
(pickset:to-list-by-ssnamex ss)
```

**参数**: 1 ss  : 选择集;

**返回值**: lst

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:to-list-by-ssnamex (ss / i lst)
  "选择集转图元表，ssnamex 方法"
  "lst"
  ""
  (setq ssx (reverse (ssnamex ss)))
  (while (< (caar ssx)
      0)
    (setq ssx (cdr ssx)))
  (setq lst (reverse (mapcar (quote cadr)
        ssx))))
```
</details>

---

### pickset:to-vlalist

**说明**: 选择集或图元列表转为Vla列表

**用法**:
```lisp
(pickset:to-vlalist ss)
```

**参数**: 1 ss  : 选择集;

**返回值**: Vla列表

**示例**:
```lisp
(pickset:to-vlalist (ssget))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:to-vlalist (ss)
  "选择集或图元列表转为Vla列表"
  "Vla列表"
  "(pickset:to-vlalist (ssget))"
  (if (= (quote pickset)
      (type ss))
    (setq ss (pickset:to-list ss)))
  (if ss (mapcar (quote vlax-ename->vla-object)
      ss)))
```
</details>

---

### pickset:zoom

**说明**: 缩放选择集ss到当前视口.显示的屏占比可由设置选项 @:magnify 控制，其值越大，放大率越低。默认满屏

**用法**:
```lisp
(pickset:zoom ss)
```

**参数**: 1 ss  : 选择集;

**示例**:
```lisp
(pickset:zoom (ssget))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun pickset:zoom (ss / box)
  "缩放选择集ss到当前视口.\n显示的屏占比可由设置选项 @:magnify 控制，\n其值越大，放大率越低。默认满屏"
  ""
  "(pickset:zoom (ssget))"
  (setq box (pickset:getbox ss 0))
  (vla-zoomcenter *acad* (apply (quote vlax-3d-point)
      (point:centroid box))
    (* (if (and (setq magnify (@:get-config (quote @:magnify)))
          (> magnify 0.01))
        magnify 1)
      (/ (apply (quote max)
          (mapcar (quote /)
            (mapcar (quote -)
              (cadr box)
              (car box))
            (getvar "SCREENSIZE")))
        0.001))))
```
</details>

---

## point

**模块说明**: 点坐标处理、变换

### point:2d->3d

**说明**: 将一维或二维点转换为3维点，若为一个数值，则将数值转为3维点

**用法**:
```lisp
(point:2d->3d pt-2d)
```

**参数**: 1 pt-2d  : 单个2D/3D坐标点;

**返回值**: 三维坐标点

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun point:2d->3d (pt-2d)
    "将一维或二维点转换为3维点，若为一个数值，则将数值转为3维点"
    "三维坐标点"
    (cond ((listp pt-2d)
            (if (= 1 (length pt-2d))
                (list (float (car pt-2d))
                    0.0 0.0)
                (if (= 2 (length pt-2d))
                    (list (float (car pt-2d))
                        (float (cadr pt-2d))
                        0.0)
                    (mapcar (quote float)
                        pt-2d))))
        ((realp pt-2d)
            (list pt-2d 0.0 0.0))
        ((intp pt-2d)
            (list (float pt-2d)
                0.0 0.0))
        (t nil)))
```
</details>

---

### point:3d->2d

**说明**: 由三维点坐标返回二维点坐标

**用法**:
```lisp
(point:3d->2d pt-3d)
```

**参数**: 1 pt-3d  : 单个2D/3D坐标点;

**返回值**: 二维坐标点

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun point:3d->2d (pt-3d)
    "由三维点坐标返回二维点坐标"
    "二维坐标点"
    (if (listp pt-3d)
        (list (float (car pt-3d))
            (float (cadr pt-3d)))))
```
</details>

---

### point:centroid

**说明**: 求多个点的几何形心

**用法**:
```lisp
(point:centroid pts)
```

**参数**: 1 pts  : 多个坐标点列表;

**返回值**: POINT

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun point:centroid (pts)
  "求多个点的几何形心"
  "POINT"
  (if (> (length pts)
      0)
    (mapcar (quote (lambda (x)
          (/ x (float (length pts)))))
      (cons (apply (quote +)
          (mapcar (quote car)
            pts))
        (cons (apply (quote +)
            (mapcar (quote cadr)
              pts))
          (if (caddr (car pts))
            (cons (apply (quote +)
                (mapcar (quote caddr)
                  pts))
              nil)))))))
```
</details>

---

### point:div

**说明**: 将 pt1 pt2 之间等分 n 份后每个点的坐标(不含pt1 pt2)

**用法**:
```lisp
(point:div pt1 pt2 n)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;2 pt2  : 单个2D/3D坐标点;3 n  : 整数;

**返回值**: 点坐标表

**示例**:
```lisp
(point:div (getpoint(@:speak"起点:")) (getpoint(@:speak"终点:")) 5)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun point:div (pt1 pt2 n / pt)
  "将 pt1 pt2 之间等分 n 份后每个点的坐标(不含pt1 pt2)"
  "点坐标表"
  "(point:div (getpoint(@:speak\"起点:\"))
    (getpoint(@:speak\"终点:\"))
    5)"
  (setq n (fix n))
  (if (and (> n 1)
      (= (quote point)
        (type-of pt1))
      (= (quote point)
        (type-of pt2)))
    (cons (setq pt (polar pt1 (angle pt1 pt2)
          (/ (distance pt1 pt2)
            (float n))))
      (div pt pt2 (1- n)))))
```
</details>

---

### point:in-box

**说明**: 判断 pt1 是否在矩形内

**用法**:
```lisp
(point:in-box pt1 pt-box1 pt-box2)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;  2 pt-box1  : 单个2D/3D坐标点;  3 pt-box2  : 单个2D/3D坐标点;

**返回值**: T or nil

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun point:in-box (pt1 pt-box1 pt-box2)
    "判断 pt1 是否在矩形内"
    "t or nil"
    (and (>= (car pt1)
            (car pt-box1))
        (<= (car pt1)
            (car pt-box2))
        (>= (cadr pt1)
            (cadr pt-box1))
        (<= (cadr pt1)
            (cadr pt-box2))))
```
</details>

---

### point:mid

**说明**: 求两点 pt1 pt2 的中点

**用法**:
```lisp
(point:mid pt1 pt2)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;  2 pt2  : 单个2D/3D坐标点;

**返回值**: 中点坐标

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun point:mid (pt1 pt2)
    "求两点 pt1 pt2 的中点"
    "中点坐标"
    (mapcar (quote (lambda (x y)
                (* 0.5 (+ x y))))
        pt1 pt2))
```
</details>

---

### point:rec-2pt->4pt

**说明**: 根据矩形2点计算矩形4点

**用法**:
```lisp
(point:rec-2pt->4pt pt1 pt2)
```

**参数**: 1 pt1  : 单个2D/3D坐标点;  2 pt2  : 单个2D/3D坐标点;

**返回值**: 矩形的四点坐标

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun point:rec-2pt->4pt (pt1 pt2)
    "根据矩形2点计算矩形4点"
    "矩形的四点坐标"
    (mapcar (quote (lambda (x)
                (mapcar (quote apply)
                    x (mapcar (quote list)
                        pt1 pt2))))
        (quote ((min min)
                (max min)
                (max max)
                (min max)))))
```
</details>

---

### point:remove-duplicates

**说明**: 去掉点列表中的重复或距离较小的的相邻点

**用法**:
```lisp
(point:remove-duplicates pts)
```

**参数**: 1 pts  : 多个坐标点列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun point:remove-duplicates (pts / lst)
  "去掉点列表中的重复或距离较小的的相邻点"
  ""
  (setq lst (cons (car pts)
      nil))
  (foreach pt pts (if (> (distance pt (car lst))
        0.0001)
      (setq lst (cons pt lst))))
  lst)
```
</details>

---

### point:to-ax

**说明**: 将点坐标转为ActiveX对象

**用法**:
```lisp
(point:to-ax pt)
```

**参数**: 1 pt  : 单个2D/3D坐标点;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun point:to-ax (pt)
  "将点坐标转为ActiveX对象"
  (apply (quote vlax-3d-point)
    pt))
```
</details>

---

## py

**模块说明**: 专用功能模块

### py:install

**说明**: 安装Python运行环境

**用法**:
```lisp
(py:install )
```

**参数**: 没有一个

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun py:install nil "安装Python运行环境"
  ""
  (or @::enable-start (@::check-pgp)
    (@::patch-pgp))
  (setvar "cmdecho"
    0)
  (@::cmd "powershell"
    "winget install python.python.3.13")
  (setvar "cmdecho"
    1))
```
</details>

---

## re

**模块说明**: 专用功能模块

### re:match

**说明**: 正则表达式匹配索字串. pattern 为以/开头及以/+修饰符结尾的字串，\ 需写成 \\ 形式。

**用法**:
```lisp
(re:match pattern string)
```

**参数**: 1 pattern  : 未识别定义;2 string  : 字符串;

**返回值**: List

**示例**:
```lisp
(re:match "/ab/g" "cabcdabcd")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun re:match (pattern string / regex s pos len str l flags)
  "正则表达式匹配索字串. pattern 为以/开头及以/+修饰符结尾的字串，\\ 需写成 \\\\ 形式。"
  "List"
  "(re:match \"/ab/g\"
    \"cabcdabcd\")"
  (setq lst-p (string:to-list pattern "/"))
  (setq flags (strcase (last lst-p)
      t))
  (setq pattern (string:from-lst (reverse (cdr (reverse (cdr lst-p))))
      "/"))
  (setq regex (vlax-create-object "Vbscript.RegExp"))
  (mapcar (quote (lambda (x)
        (vlax-put-property regex x 0)))
    (quote ("Global"
        "IgnoreCase"
        "Multiline")))
  (foreach f (vl-string->list flags)
    (cond ((= f (ascii "g"))
        (vlax-put-property regex "Global"
          1))
      ((= f (ascii "i"))
        (vlax-put-property regex "IgnoreCase"
          1))
      ((= f (ascii "m"))
        (vlax-put-property regex "Multiline"
          1))))
  (vlax-put-property regex "Pattern"
    pattern)
  (setq s (vlax-invoke-method regex (quote execute)
      string))
  (vlax-for o s (setq str (vlax-get-property o "value"))
    (setq l (cons str l)))
  (vlax-release-object regex)
  (reverse l))
```
</details>

---

### re:replace

**说明**: 正则表达式替换字符串. pattern 为以/开头及以/+修饰符结尾的字串，\ 需写成 \\ 形式。

**用法**:
```lisp
(re:replace pattern string newstr)
```

**参数**: 1 pattern  : 未识别定义;2 string  : 字符串;3 newstr  : 未识别定义;

**返回值**: String

**示例**:
```lisp
(re:replace "/ab/g" "cabcdabcd" "mm")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun re:replace (pattern string newstr / regex s pos len str l flags)
  "正则表达式替换字符串. pattern 为以/开头及以/+修饰符结尾的字串，\\ 需写成 \\\\ 形式。"
  "String"
  "(re:replace \"/ab/g\"
    \"cabcdabcd\"
    \"mm\")"
  (setq lst-p (string:to-list pattern "/"))
  (setq flags (strcase (last lst-p)
      t))
  (setq pattern (string:from-lst (reverse (cdr (reverse (cdr lst-p))))
      "/"))
  (setq regex (vlax-create-object "Vbscript.RegExp"))
  (mapcar (quote (lambda (x)
        (vlax-put-property regex x 0)))
    (quote ("Global"
        "IgnoreCase"
        "Multiline")))
  (foreach f (vl-string->list flags)
    (cond ((= f (ascii "g"))
        (vlax-put-property regex "Global"
          1))
      ((= f (ascii "i"))
        (vlax-put-property regex "IgnoreCase"
          1))
      ((= f (ascii "m"))
        (vlax-put-property regex "Multiline"
          1))))
  (vlax-put-property regex "Pattern"
    pattern)
  (setq s (vlax-invoke-method regex (quote replace)
      string newstr))
  (vlax-release-object regex)
  s)
```
</details>

---

### re:search

**说明**: 正则表达式搜索字串. pattern 为以/开头及以/+修饰符结尾的字串，\ 需写成 \\ 形式。

**用法**:
```lisp
(re:search pattern string)
```

**参数**: 1 pattern  : 未识别定义;2 string  : 字符串;

**返回值**: list

**示例**:
```lisp
(re:search "/ab/g" "cabcdabcd")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun re:search (pattern string / regex s pos len str l flags)
  "正则表达式搜索字串. pattern 为以/开头及以/+修饰符结尾的字串，\\ 需写成 \\\\ 形式。"
  "list"
  "(re:search \"/ab/g\"
    \"cabcdabcd\")"
  (setq lst-p (string:to-list pattern "/"))
  (setq flags (strcase (last lst-p)
      t))
  (setq pattern (string:from-lst (reverse (cdr (reverse (cdr lst-p))))
      "/"))
  (setq regex (vlax-create-object "Vbscript.RegExp"))
  (mapcar (quote (lambda (x)
        (vlax-put-property regex x 0)))
    (quote ("Global"
        "IgnoreCase"
        "Multiline")))
  (foreach f (vl-string->list flags)
    (cond ((= f (ascii "g"))
        (vlax-put-property regex "Global"
          1))
      ((= f (ascii "i"))
        (vlax-put-property regex "IgnoreCase"
          1))
      ((= f (ascii "m"))
        (vlax-put-property regex "Multiline"
          1))))
  (vlax-put-property regex "Pattern"
    pattern)
  (setq s (vlax-invoke-method regex (quote execute)
      string))
  (vlax-for o s (setq pos (vlax-get-property o "FirstIndex")
      len (vlax-get-property o "Length")
      str (vlax-get-property o "value"))
    (setq l (cons (list pos len str)
        l)))
  (vla:dump regex)
  (vlax-release-object regex)
  (reverse l))
```
</details>

---

## reg

**模块说明**: 专用功能模块

### reg:list-app

**说明**: 列出vlax-get-or-create-object可用的应用对象

**用法**:
```lisp
(reg:list-app )
```

**参数**: 没有一个

**返回值**: lst

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun reg:list-app nil "列出vlax-get-or-create-object可用的应用对象 "
  "lst"
  (setq axroot "HKEY_LOCAL_MACHINE\\SOFTWARE\\Classes\\WOW6432Node\\CLSID")
  (setq clsids (vl-remove-if-not (quote (lambda (x / ax)
          (setq sub (mapcar (quote strcase)
              (vl-registry-descendents (strcat axroot "\\"
                  X))))
          (AND (MEMBER (STRCASE "inprocserver32")
              SUB)
            (MEMBER (STRCASE "progid")
              SUB))))
      (VL-REGISTRY-DESCENDENTS AXROOT)))
  (MAPCAR (QUOTE (LAMBDA (X)
        (VL-REGISTRY-READ (STRCAT AXROOT "\\"
            x "\\ProgID"))))
    clsids))
```
</details>

---

## stat

**模块说明**: 专用功能模块

### stat:classify

**说明**: 分类统计由一系列类别和数字组成的点对表。按类别统计求类别后数值的总和。

**用法**:
```lisp
(stat:classify lst-cons)
```

**参数**: 1 lst-cons  : 列表;

**返回值**: lst

**示例**:
```lisp
(stat:classify '((a . 5)(b . 3)(a . 2)(b . 6)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun stat:classify (lst-cons / res)
  "分类统计由一系列类别和数字组成的点对表。按类别统计求类别后数值的总和。"
  "lst"
  "(stat:classify '((a . 5)(b . 3)(a . 2)(b . 6)))"
  (setq res nil)
  (foreach cons% lst-cons (if (assoc (car cons%)
        res)
      (setq res (subst (cons (car cons%)
            (+ (cdr (assoc (car cons%)
                  res))
              (cdr cons%)))
          (assoc (car cons%)
            res)
          res))
      (setq res (cons cons% res))))
  res)
```
</details>

---

### stat:draw

**说明**: 绘制最后一次统计的结果

**用法**:
```lisp
(stat:draw )
```

**参数**: 没有一个

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun stat:draw (/ n pt ent-table *error*)
  "绘制最后一次统计的结果"
  (defun *error* (msg)
    (if ent-table (entdel ent-table))
    (princ msg))
  (if @:tmp-stat-result (progn (setq pt (quote (0 0 0)))
      (setq n 0)
      (setq ent-table (table:make pt "统计结果"
          (quote ("项目"
              "个数"))
          (mapcar (quote (lambda (x)
                (list (car x)
                  (cdr x))))
            @:tmp-stat-result)))
      (ui:dyndraw ent-table pt))))
```
</details>

---

### stat:mode

**说明**: 众数

**用法**:
```lisp
(stat:mode stat-res)
```

**参数**: 1 stat-res  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun stat:mode (stat-res)
  "众数"
  (car (vl-sort stat-res (function (lambda (e1 e2)
          (> (cdr e1)
            (cdr e2)))))))
```
</details>

---

### stat:print

**说明**: 打印最后一次统计的结果

**用法**:
```lisp
(stat:print )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun stat:print nil "打印最后一次统计的结果"
  (princ "Item , Number\n")
  (foreach n @:tmp-stat-result (princ (car n))
    (princ "
      , ")
    (princ (cdr n))
    (princ "\n")))
```
</details>

---

### stat:stat

**说明**: 统计列表 lst 中的元素个数。

**用法**:
```lisp
(stat:stat lst)
```

**参数**: 1 lst  : 列表;

**返回值**: 元素和个数组成的点对表

**示例**:
```lisp
(stat:stat '(3 a a 2 2)) => ((3 . 1) (A . 2) (2 . 2))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun stat:stat (lst / atom% res)
  "统计列表 lst 中的元素个数。"
  "元素和个数组成的点对表"
  "(stat:stat '(3 a a 2 2))
  => ((3 . 1)
    (A . 2)
    (2 . 2))"
  (setq res (quote nil))
  (foreach atom% lst (if (assoc atom% res)
      (setq res (subst (cons atom% (+ 1 (cdr (assoc atom% res))))
          (assoc atom% res)
          res))
      (setq res (append res (list (cons atom% 1))))))
  res)
```
</details>

---

## std

**模块说明**: 专用功能模块

### std:acad-object

**说明**: 返回CAD对象

**用法**:
```lisp
(std:acad-object )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:acad-object nil "返回cad对象"
    (eval (list (quote defun)
            (quote std:acad-object)
            (quote nil)
            (vlax-get-acad-object)))
    (std:acad-object))
```
</details>

---

### std:active-document

**说明**: 返回当前活动文档对象

**用法**:
```lisp
(std:active-document )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:active-document nil "返回当前活动文档对象"
    (eval (list (quote defun)
            (quote std:active-document)
            (quote nil)
            (vla-get-activedocument (vlax-get-acad-object))))
    (std:active-document))
```
</details>

---

### std:addmenu

**用法**:
```lisp
(std:addmenu menugroupname popname popitems insertbeforeitem)
```

**参数**: 1 menugroupname  : 未明确定义;  2 popname  : 未明确定义;  3 popitems  : 未明确定义;  4 insertbeforeitem  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:addmenu (menugroupname popname popitems insertbeforeitem / i menubar menuitem n popupmenu)
    (std:removemenuitem popname)
    (setq menubar (vla-get-menubar (vlax-get-acad-object)))
    (if insertbeforeitem (progn (setq n (vla-get-count menubar))
            (setq i (1- n))
            (while (and (>= i 0)
                    (/= insertbeforeitem (vla-get-name (setq menuitem (vla-item menubar i)))))
                (setq i (1- i)))
            (if (< i 0)
                (setq i (vla-get-count menubar))))
        (setq i (vla-get-count menubar)))
    (if (not (setq popupmenu (std:catchapply (quote vla-item)
                    (list (vla-get-menus (vla-item (vla-get-menugroups (vlax-get-acad-object))
                                menugroupname))
                        popname))))
        (setq popupmenu (vla-add (vla-get-menus (vla-item (vla-get-menugroups (vlax-get-acad-object))
                        menugroupname))
                popname)))
    (vlax-for popupmenuitem popupmenu (vla-delete popupmenuitem))
    (vla-insertinmenubar popupmenu i)
    (std:insertpopmenuitems popupmenu popitems)
    (princ))
```
</details>

---

### std:addsupportpath

**用法**:
```lisp
(std:addsupportpath lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:addsupportpath (lst)
    ((lambda (str lst)
            (if (setq lst (vl-remove-if (quote (lambda (x)
                                (or (vl-string-search (strcase x)
                                        (strcase str))
                                    (not (findfile x)))))
                        lst))
                (setenv "acad"
                    (strcat str ";"
                        (apply (quote strcat)
                            (mapcar (quote (lambda (x)
                                        (strcat x ";")))
                                lst))))))
        (vl-string-right-trim ";"
            (getenv "acad"))
        (mapcar (quote (lambda (x)
                    (vl-string-right-trim "\\"
                        (vl-string-translate "/"
                            "\\"
                            x))))
            lst)))
```
</details>

---

### std:addtoolbars

**用法**:
```lisp
(std:addtoolbars menugroupname toolbaritems)
```

**参数**: 1 menugroupname  : 未明确定义;  2 toolbaritems  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:addtoolbars (menugroupname toolbaritems / flyout flyoutbutton helpstring idx items largeiconname left macro menugroupobj name smalliconname toolbar toolbaritem toolbarname toolbars top)
    (if (not (setq menugroupobj (std:catchapply vla-item (list (vla-get-menugroups (vlax-get-acad-object))
                        menugroupname))))
        (progn (alert (strcat "菜单组\""
                    menugroupname "\"不存在！无法加载菜单条！"))
            (exit)))
    (setq toolbars (vla-get-toolbars menugroupobj))
    (foreach items toolbaritems (setq toolbarname (car items)
            left (cadr items)
            top (caddr items)
            items (cdddr items))
        (if (setq toolbar (std:catchapply vla-item (list toolbars toolbarname)))
            (vla-delete toolbar))
        (setq toolbar (vla-add toolbars toolbarname))
        (vla-put-left toolbar left)
        (vla-put-top toolbar top)
        (setq idx 0)
        (foreach lst items (setq name (car lst)
                helpstring (cadr lst)
                macro (caddr lst)
                smalliconname (cadddr lst)
                largeiconname (car (cddddr lst))
                flyoutbutton (cadr (cddddr lst)))
            (if (not largeiconname)
                (setq largeiconname smalliconname))
            (if flyoutbutton (setq flyout :vlax-true)
                (setq flyout :vlax-false))
            (setq toolbaritem (std:catchapply vla-addtoolbarbutton (list toolbar idx name helpstring macro flyout)))
            (std:catchapply vla-setbitmaps (list toolbaritem smalliconname largeiconname))
            (if flyoutbutton (std:catchapply vla-attachtoolbartoflyout (list toolbaritem menugroupname flyoutbutton)))
            (setq idx (1+ idx)))))
```
</details>

---

### std:catchapply

**用法**:
```lisp
(std:catchapply fun args)
```

**参数**: 1 fun  : 未明确定义;  2 args  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:catchapply (fun args / result)
    (if (not (vl-catch-all-error-p (setq result (vl-catch-all-apply (if (= (quote sym)
                                (type fun))
                            fun (function fun))
                        args))))
        result))
```
</details>

---

### std:e->vla

**用法**:
```lisp
(std:e->vla ename)
```

**参数**: 1 ename  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:e->vla (ename)
    (vlax-ename->vla-object ename))
```
</details>

---

### std:endundo

**用法**:
```lisp
(std:endundo doc)
```

**参数**: 1 doc  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:endundo (doc)
    (while (= 8 (logand 8 (getvar (quote undoctl))))
        (vla-endundomark doc)))
```
</details>

---

### std:getinput

**说明**: 获取输入，结合initget和getkword函数

**用法**:
```lisp
(std:getinput promptstr inplist default)
```

**参数**: 1 promptstr  : 未识别定义;2 inplist  : 未识别定义;3 default  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:getinput (promptstr inplist default / inp)
  "获取输入，结合initget和getkword函数"
  (initget (if default 0 1)
    (string:from-list inplist "\n            "))
  (if (setq inp (getkword (strcat (if promptstr (strcat promptstr "\n                            [")
            "[")
          (string:from-list inplist "/")
          "]"
          (if (and default (member default inplist))
            (strcat "\n                            <"
              default ">: ")
            ": "))))
    inp default))
```
</details>

---

### std:insertpopmenuitems

**用法**:
```lisp
(std:insertpopmenuitems popupmenu popitems)
```

**参数**: 1 popupmenu  : 未明确定义;  2 popitems  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:insertpopmenuitems (popupmenu popitems / k tmp)
    (setq k 0)
    (mapcar (function (lambda (x / label cmdstr hlpstr subitems tmp)
                (setq label (car x)
                    cmdstr (cadr x)
                    hlpstr (caddr x)
                    subitems (cadddr x))
                (if (= label "--")
                    (vla-addseparator popupmenu (setq k (1+ k)))
                    (if (and label cmdstr)
                        (progn (setq tmp (vla-addmenuitem popupmenu (setq k (1+ k))
                                    label cmdstr))
                            (vla-put-helpstring tmp hlpstr))
                        (progn (setq tmp (vla-addsubmenu popupmenu (setq k (1+ k))
                                    label))
                            (if subitems (std:insertpopmenuitems tmp subitems)))))))
        popitems))
```
</details>

---

### std:layers

**说明**: 返回图层集合

**用法**:
```lisp
(std:layers )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:layers nil "返回图层集合"
    (eval (list (quote defun)
            (quote std:layers)
            (quote nil)
            (vla-get-layers (vla-get-activedocument (vlax-get-acad-object)))))
    (std:layers))
```
</details>

---

### std:linetypes

**说明**: 返回线型集合

**用法**:
```lisp
(std:linetypes )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:linetypes nil "返回线型集合"
    (eval (list (quote defun)
            (quote std:line-types)
            (quote nil)
            (vla-get-linetypes (vla-get-activedocument (vlax-get-acad-object)))))
    (std:line-types))
```
</details>

---

### std:model-space

**说明**: 返回模型空间对象

**用法**:
```lisp
(std:model-space )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:model-space nil "返回模型空间对象"
    (eval (list (quote defun)
            (quote std:model-space)
            (quote nil)
            (vla-get-modelspace (vla-get-activedocument (vlax-get-acad-object)))))
    (std:model-space))
```
</details>

---

### std:osmode-off

**说明**: 关闭对象捕捉，不影响对象捕捉设置。

**用法**:
```lisp
(std:osmode-off )
```

**参数**: 没有

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:osmode-off nil "关闭对象捕捉，不影响对象捕捉设置。"
  ""
  (setvar "osmode"
    (boole 7 (getvar "osmode")
      16384)))
```
</details>

---

### std:osmode-on

**说明**: 打开对象捕捉，不影响对象捕捉设置。

**用法**:
```lisp
(std:osmode-on )
```

**参数**: 没有

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:osmode-on nil "打开对象捕捉，不影响对象捕捉设置。"
  ""
  (setvar "osmode"
    (- (boole 7 (getvar "osmode")
        16384)
      16384)))
```
</details>

---

### std:protect-assign

**用法**:
```lisp
(std:protect-assign syms)
```

**参数**: 1 syms  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:protect-assign (syms)
    (eval (list (quote pragma)
            (list (quote quote)
                (list (cons (quote protect-assign)
                        syms))))))
```
</details>

---

### std:removemenuitem

**用法**:
```lisp
(std:removemenuitem popname)
```

**参数**: 1 popname  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:removemenuitem (popname / menubar menuitem)
    (setq menubar (vla-get-menubar (vlax-get-acad-object)))
    (setq menuitem (std:catchapply (quote vla-item)
            (list menubar popname)))
    (if menuitem (std:catchapply (quote vla-removefrommenubar)
            (list menuitem))))
```
</details>

---

### std:removesupportpath

**用法**:
```lisp
(std:removesupportpath lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:removesupportpath (lst / del str tmp)
    (defun del (old str / pos)
        (if (setq pos (vl-string-search (strcase old)
                    (strcase str)))
            (strcat (substr str 1 pos)
                (del old (substr str (+ 1 pos (strlen old)))))
            str))
    (setq str (strcat (vl-string-right-trim ";"
                (getenv "acad"))
            ";")
        tmp str)
    (foreach pth lst (setq str (del (strcat (vl-string-right-trim "\\"
                        (vl-string-translate "/"
                            "\\"
                            pth))
                    ";")
                str)))
    (if (/= tmp str)
        (setenv "acad"
            str)))
```
</details>

---

### std:reset-system-variable

**用法**:
```lisp
(std:reset-system-variable )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:reset-system-variable nil (mapcar (quote setvar)
        (mapcar (quote car)
            *user-system-variable*)
        (mapcar (quote cdr)
            *user-system-variable*))
    (setq *user-system-variable* nil))
```
</details>

---

### std:return

**说明**: 返回值函数，用于包装将要返回的值，主要作用还是为了含义更明确。

**用法**:
```lisp
(std:return value)
```

**参数**: 1 value  : 值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:return (value)
    "返回值函数，用于包装将要返回的值，主要作用还是为了含义更明确。"
    value)
```
</details>

---

### std:rgb

**说明**: 计算RGB颜色对应的整数值。Red Green Blue 取值范围为 [0,255]的整数或[0,1)的小数。

**用法**:
```lisp
(std:rgb red green blue)
```

**参数**: 1 red  : 未明确定义;  2 green  : 未明确定义;  3 blue  : 未明确定义;

**返回值**: RGB颜色值

**示例**:
```lisp
(std:rgb 255 0 0) or (std:rgb 0.999 0 0);  红色
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:rgb (red green blue)
    "计算rgb颜色对应的整数值。red green blue 取值范围为 [0,255]的整数或[0,1)的小数。"
"rgb颜色值"
"(std:rgb 255 0 0)
or (std:rgb 0.999 0 0);红色"
(cond ((and (<= 0 red)
            (< red 1)
            (<= 0 green)
            (< green 1)
            (<= 0 blue)
            (< blue 1))
        (+ (fix (* 256 red))
            (* 256 (fix (* 256 green)))
            (* 65536 (fix (* 256 blue)))))
    ((and (<= 0 red 255)
            (<= 0 green 255)
            (<= 0 blue 255))
        (+ (fix red)
            (* 256 (fix green))
            (* 65536 (fix blue))))
    (t (+ 255 (* 256 255)
            (* 65536 255)))))
```
</details>

---

### std:save-system-variable

**用法**:
```lisp
(std:save-system-variable a)
```

**参数**: 1 a  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:save-system-variable (a)
    (setq *user-system-variable* (mapcar (quote cons)
            a (mapcar (quote getvar)
                a))))
```
</details>

---

### std:startundo

**用法**:
```lisp
(std:startundo doc)
```

**参数**: 1 doc  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:startundo (doc)
    (std:endundo doc)
    (vla-startundomark doc))
```
</details>

---

### std:tbl-rename

**说明**: 重命名DXF表格中的项目的名称。 表格为:block,dimstyle,layer,layout,linetype,textstyle,view,vports.   参数:tbl-name tbl表格名, old-name 原名称，new-name 新名称

**用法**:
```lisp
(std:tbl-rename tbl-name old-name new-name)
```

**参数**: 1 tbl-name  : 未识别定义;2 old-name  : 未识别定义;3 new-name  : 未识别定义;

**返回值**: T 成功，nil 失败

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:tbl-rename (tbl-name old-name new-name)
  "重命名DXF表格中的项目的名称。 表格为:block,dimstyle,layer,layout,linetype,textstyle,view,vports. \n  参数:tbl-name tbl表格名, old-name 原名称，new-name 新名称"
  "T 成功，nil 失败"
  ""
  (defun tbl-dxf2obj (tbl-name)
    (cond ((string-equal tbl-name "LTYPE")
        "linetypes")
      ((string-equal tbl-name "STYLE")
        "textstyles")
      ((string-equal tbl-name "UCS")
        "UserCoordinateSystems")
      ((string-equal tbl-name "VPORT")
        "Viewports")
      (t (strcat tbl-name "s"))))
  (if (and (tblsearch tbl-name old-name)
      (not (tblsearch tbl-name new-name))
      (snvalid new-name))
    (progn (if (vl-catch-all-error-p (vl-catch-all-apply (quote vla-put-name)
            (list (vla-item (vlax-get-property *doc* (read (strcat (tbl-dxf2obj tbl-name))))
                old-name)
              new-name)))
        nil t))))
```
</details>

---

### std:textstyles

**说明**: 返回字体样式集合

**用法**:
```lisp
(std:textstyles )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:textstyles nil "返回字体样式集合"
    (eval (list (quote defun)
            (quote std:textstyles)
            (quote nil)
            (vla-get-textstyles (vla-get-activedocument (vlax-get-acad-object)))))
    (std:textstyles))
```
</details>

---

### std:timer-end

**说明**: 计时器结束函数

**用法**:
```lisp
(std:timer-end )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:timer-end nil "计时器结束函数"
    (princ "\n    用时")
    (princ (* (- (getvar "tdusrtimer")
                @:*timer-prg*)
            86400))
    (princ "秒\n")
    (setq @:*timer-prg* nil)
    (princ))
```
</details>

---

### std:timer-start

**说明**: 计时器开始函数

**用法**:
```lisp
(std:timer-start )
```

**参数**: None

**返回值**: 计时器全局变量

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:timer-start nil "计时器开始函数"
    "计时器全局变量"
    (setq @:*timer-prg* (getvar "tdusrtimer")))
```
</details>

---

### std:unprotect-assign

**用法**:
```lisp
(std:unprotect-assign syms)
```

**参数**: 1 syms  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:unprotect-assign (syms)
    (eval (list (quote pragma)
            (list (quote quote)
                (list (cons (quote unprotect-assign)
                        syms))))))
```
</details>

---

### std:vla->e

**用法**:
```lisp
(std:vla->e obj)
```

**参数**: 1 obj  : activeX 对象;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun std:vla->e (obj)
    (vlax-vla-object->ename obj))
```
</details>

---

## string

**模块说明**: 字符串处理、格式化、编码转换

### string:align-by-length

**说明**: 将字节长度小于 len 的字符串前后增加空格，使其长度等于 len。可用于DCL中的字符对齐。

**用法**:
```lisp
(string:align-by-length str len)
```

**参数**: 1 str  : 字符串;2 len  : 未识别定义;

**返回值**: String

**示例**:
```lisp
(string:align-by-length "abc" 8) => "   abc  "
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:align-by-length (str len / flag)
  "将字节长度小于 len 的字符串前后增加空格，使其长度等于 len。可用于DCL中的字符对齐。"
  "String"
  "(string:align-by-length \"abc\"
    8)
  => \"
    abc  \""
  (if (null str)
    (setq str ""))
  (setq flag nil)
  (while (< (string:bytelength str)
      len)
    (if flag (setq str (strcat "
          "
          str))
      (setq str (strcat str "
          ")))
    (setq flag (not flag)))
  str)
```
</details>

---

### string:auto-split

**说明**: 自动分段，按数字-字母-汉字自动断开字符串为字符串列表。不支持科学计数法的数字。

**用法**:
```lisp
(string:auto-split str)
```

**参数**: 1 str  : 字符串;

**返回值**: 由字符串组成的列表

**示例**:
```lisp
(string:auto-split "aa33.3bb汉字")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:auto-split (str / curr-type lst tmp% res unitp)
  "自动分段，按数字-字母-汉字自动断开字符串为字符串列表。不支持科学计数法的数字。"
  "由字符串组成的列表"
  "(string:auto-split \"aa33.3bb汉字\")"
  (defun is-bracket (asc)
    (member asc (vl-string->list "()")))
  (defun is-operator (asc)
    (member asc (vl-string->list "+-*/·")))
  (defun is-unitsuffix (asc)
    (member asc (list 178 179)))
  (defun is-unitprefix (asc)
    (member asc (list 181)))
  (defun is-number (asc)
    (or (= asc 46)
      (and (>= asc 48)
        (<= asc 57))))
  (defun is-alpha (asc)
    (or (and (>= asc 65)
        (<= asc 90))
      (and (>= asc 97)
        (<= asc 122))))
  (defun is-hannum (asc)
    (member asc (string:s2l-ansi "零一二三四五六七六九十百千万亿壹贰叁肆伍陆柒捌玖拾佰仟")))
  (defun is-han (asc)
    (if (and (getvar "lispsys")
        (> (getvar "lispsys")
          0))
      (>= asc 19968)
      (>= asc 32896)))
  (defun is-sign (asc)
    (or (= asc (ascii "+"))
      (= asc (ascii "-"))))
  (defun test-type (asc)
    (cond ((is-number asc)
        (quote number))
      ((is-alpha asc)
        (quote alpha))
      ((is-hannum asc)
        (quote hannumber))
      ((is-han asc)
        (quote han))
      ((is-sign asc)
        (quote sign))
      ((is-bracket asc)
        (quote bracket))
      ((is-operator asc)
        (quote operator))
      ((is-unitsuffix asc)
        (quote unitsuffix))
      ((is-unitprefix asc)
        (quote unitprefix))
      (t nil)))
  (setq res (mapcar (quote (lambda (x)
          (cons x (test-type x))))
      (string:s2l-ansi str)))
  (setq curr-type (cdr (car res)))
  (setq lst (quote nil))
  (foreach asc res (cond ((or (= curr-type (cdr asc))
          (and (= curr-type (quote sign))
            (= (cdr asc)
              (quote number))))
        (setq tmp% (cons (car asc)
            tmp%)))
      (t (progn (setq lst (cons (reverse tmp%)
              lst))
          (setq tmp% (cons (car asc)
              nil))
          (setq curr-type (cdr asc))))))
  (setq lst (reverse (cons (reverse tmp%)
        lst)))
  (setq res (mapcar (quote string:l2s-ansi)
      lst))
  (setq unitp (member (car res)
      @:*units*))
  (setq lst nil)
  (setq tmp% "")
  (foreach word res (cond ((and unitp (member word @:*units*))
        (setq tmp% (strcat tmp% word)))
      (t (progn (if (/= ""
              tmp%)
            (setq lst (cons tmp% lst)))
          (setq tmp% word)
          (setq unitp (member word @:*units*))))))
  (setq lst (reverse (cons tmp% lst)))
  lst)
```
</details>

---

### string:bytelength

**说明**: 字符串的字节数，用于cad2021版本。

**用法**:
```lisp
(string:bytelength str)
```

**参数**: 1 str  : 字符串;

**返回值**: 字符串的字节数

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:bytelength (str)
  "字符串的字节数，用于cad2021版本。"
  "字符串的字节数"
  (+ (length (vl-string->list str))
    (length (vl-remove-if (quote (lambda (m)
            (< m 256)))
        (vl-string->list str)))))
```
</details>

---

### string:case

**说明**: 大小写替换

**用法**:
```lisp
(string:case str)
```

**参数**: 1 str  : 字符串;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:case (str)
  "大小写替换"
  (vl-list->string (mapcar (quote (lambda (x)
          (cond ((and (>= x 65)
                (< x 97))
              (+ x 32))
            ((and (>= x 97)
                (< x 123))
              (- x 32)))))
      (vl-string->list str))))
```
</details>

---

### string:concat

**说明**: 连接字符串，连接前进行检测。

**用法**:
```lisp
(string:concat strlst)
```

**参数**: 1 strlst  : 字符串;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:concat (strlst)
  "连接字符串，连接前进行检测。"
  (cond ((std:stringp strlst)
      strlst)
    ((std:string-listp strlst)
      (apply (quote strcat)
        strlst))
    (t nil)))
```
</details>

---

### string:escape

**说明**: 对字符串 str 中的 chars-need-to 字符进行转义，转义符为escape-chr。如果需转义的字符中含转义符，需将其放在首位。

**用法**:
```lisp
(string:escape str escape-chr chars-need-to)
```

**参数**: 1 str  : 字符串;2 escape-chr  : 未识别定义;3 chars-need-to  : 未识别定义;

**返回值**: string

**示例**:
```lisp
(string:escape "@*" "`" "@*") => "`@`*"
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:escape (str escape-chr chars-need-to)
  "对字符串 str 中的 chars-need-to 字符进行转义，转义符为escape-chr。如果需转义的字符中含转义符，需将其放在首位。"
  "string"
  "(string:escape \"@*\"
    \"`\"
    \"@*\")
  => \"`@`*\""
  (foreach chr (string:s2l-ansi chars-need-to)
    (setq str (string:subst-all (strcat escape-chr (string:l2s-ansi (list chr)))
        (string:l2s-ansi (list chr))
        str))))
```
</details>

---

### string:format

**说明**: 字符串格式化函数

**用法**:
```lisp
(string:format str formatlist)
```

**参数**: 1 str  : 字符串;2 formatlist  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:format (str formatlist / i str-length)
  "字符串格式化函数"
  (if (std:stringp formatlist)
    (setq str (string:subst-all formatlist "{0}"
        str))
    (progn (setq i -1)
      (repeat (vl-list-length formatlist)
        (setq str (string:subst-all (nth (setq i (1+ i))
              formatlist)
            (strcat "{"
              (itoa i)
              "}")
            str))))))
```
</details>

---

### string:from-list

**说明**: 列表转成字符串

**用法**:
```lisp
(string:from-list lst separator)
```

**参数**: 1 lst  : 列表;2 separator  : 分隔符;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:from-list (lst separator)
  "列表转成字符串"
  (if (cdr lst)
    (strcat (car lst)
      separator (string:from-lst (cdr lst)
        separator))
    (car lst)))
```
</details>

---

### string:from-lst

**说明**: 列表转成字符串

**用法**:
```lisp
(string:from-lst lst separator)
```

**参数**: 1 lst  : 列表;2 separator  : 分隔符;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:from-lst (lst separator)
  "列表转成字符串"
  (if (cdr lst)
    (strcat (car lst)
      separator (string:from-lst (cdr lst)
        separator))
    (car lst)))
```
</details>

---

### string:hannumber2number

**说明**: 中文数字转为数字类型

**用法**:
```lisp
(string:hannumber2number str)
```

**参数**: 1 str  : 字符串;

**返回值**: number

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:hannumber2number (str / numlst alst1 alst2 num lnum1 lnum2 fstr a)
  "中文数字转为数字类型"
  "number"
  (setq numlst (quote (("零"
          . 0)
        ("一"
          . 1)
        ("壹"
          . 1)
        ("二"
          . 2)
        ("贰"
          . 2)
        ("三"
          . 3)
        ("叁"
          . 3)
        ("四"
          . 4)
        ("肆"
          . 4)
        ("五"
          . 5)
        ("伍"
          . 5)
        ("六"
          . 6)
        ("陆"
          . 6)
        ("七"
          . 7)
        ("柒"
          . 7)
        ("八"
          . 8)
        ("捌"
          . 8)
        ("九"
          . 9)
        ("玖"
          . 9)))
    alst1 (quote (("十"
          . 10)
        ("拾"
          . 10)
        ("百"
          . 100)
        ("佰"
          . 100)
        ("千"
          . 1000.0)
        ("仟"
          . 1000.0)))
    alst2 (quote (("万"
          . 10000.0)
        ("亿"
          . 1.0e+08)))
    num 0 lnum1 0 lnum2 0)
  (foreach word (mapcar (quote (lambda (x)
          (string:l2s-ansi (list x))))
      (string:s2l-ansi str))
    (cond ((setq a (assoc word alst2))
        (setq num (+ num (* (+ lnum1 lnum2)
              (cdr a)))
          lnum1 0 lnum2 0))
      ((setq a (assoc word alst1))
        (if (and (zerop lnum2)
            (zerop num))
          (setq num (cdr a))
          (setq lnum1 (+ lnum1 (* lnum2 (cdr a)))
            lnum2 0)))
      ((setq a (assoc word numlst))
        (setq lnum2 (cdr a)))
      (t nil)))
  (+ num lnum1 lnum2))
```
</details>

---

### string:hannumberp

**说明**: 确定字符串是否为中文数字

**用法**:
```lisp
(string:hannumberp str)
```

**参数**: 1 str  : 字符串;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:hannumberp (str)
  "确定字符串是否为中文数字"
  (apply (quote and)
    (mapcar (quote (lambda (x)
          (member x (string:s2l-ansi "零一二三四五六七六九十百千万亿壹贰叁肆伍陆柒捌玖拾佰仟"))))
      (string:s2l-ansi str))))
```
</details>

---

### string:indent

**说明**: 缩进 lisp 代码

**用法**:
```lisp
(string:indent str)
```

**参数**: 1 str  : 字符串;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:indent (str / curr-ind% res)
  "缩进 lisp 代码"
  (setq curr-ind% 0)
  (setq res (quote nil))
  (setq str (@:string-subst "\"\n"
      "\"
      "
      (@:string-subst ")\n"
      ")
    "
    str)))
(foreach char% (vl-string->list str)
(cond ((= 40 char%)
    (setq curr-ind% (1+ curr-ind%)))
  ((= 41 char%)
    (setq curr-ind% (1- curr-ind%))))
(setq res (cons char% res))
(if (= 10 char%)
  (repeat (* 4 curr-ind%)
    (setq res (cons 32 res)))))
(vl-list->string (reverse res)))
```
</details>

---

### string:intp

**说明**: 确定字符串是否为整数

**用法**:
```lisp
(string:intp str)
```

**参数**: 1 str  : 字符串;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:intp (str)
  "确定字符串是否为整数"
  (and (string:numberp str)
    (not (or (= "."
          (substr str 1 1))
        (= "-."
          (substr str 1 2))
        (= "+."
          (substr str 1 2))))
    (= (quote int)
      (type (read str)))))
```
</details>

---

### string:l2s-ansi

**说明**: byte or word 整数值列表转字符串。n当小于128时，单字节，当两个连续的大于128时，双字节值。用于转换非英文字串时防止重码。n当AutoCAD2021且lispsys=1时，与string:s2l-ansi 成对兼容 unicode.

**用法**:
```lisp
(string:l2s-ansi lst-str)
```

**参数**: 1 lst-str  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:l2s-ansi (lst-str / h% res)
  "byte or word 整数值列表转字符串。\n当小于128时，单字节，当两个连续的大于128时，双字节值。用于转换非英文字串时防止重码。\n当AutoCAD2021且lispsys=1时，与string:s2l-ansi 成对兼容 unicode."
  (if (and (getvar "lispsys")
      (<= 1 (getvar "lispsys")))
    (vl-list->string lst-str)
    (progn (setq h% 0)
      (setq res (quote nil))
      (foreach word% lst-str (cond ((>= word% 32896)
            (setq res (cons (/ word% 256)
                res))
            (setq res (cons (rem word% 256)
                res)))
          ((< word% 32896)
            (cond ((= 178 word%)
                (setq res (append (reverse (list 92 85 43 48 48 66 50))
                    res)))
              ((= 179 word%)
                (setq res (append (reverse (list 92 85 43 48 48 66 51))
                    res)))
              (t (setq res (cons word% res)))))))
      (vl-list->string (reverse res)))))
```
</details>

---

### string:length

**说明**: 字符串长度，一个汉字占1位。

**用法**:
```lisp
(string:length str)
```

**参数**: 1 str  : 字符串;

**返回值**: int

**示例**:
```lisp
(string:length "中国a") => 3
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:length (str)
  "字符串长度，一个汉字占1位。"
  "int"
  "(string:length \"中国a\")
  => 3"
  (length (string:s2l-ansi str)))
```
</details>

---

### string:lsubstr

**说明**: 从左侧求子串

**用法**:
```lisp
(string:lsubstr str len)
```

**参数**: 1 str  : 字符串;2 len  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:lsubstr (str len)
  "从左侧求子串"
  (substr str 1 len))
```
</details>

---

### string:num-compare

**说明**: 字符串在夹杂数字时，如果数字前后的字符串相同，按数字大小比较。fun 可用函数为 >, >=, <, <=,2020-03-31

**用法**:
```lisp
(string:num-compare fun str1 str2)
```

**参数**: 1 fun  : 未识别定义;2 str1  : 字符串;3 str2  : 字符串;

**返回值**: T or nil

**示例**:
```lisp
(string:num-compare < "a5"  "a13")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:num-compare (fun str1 str2)
  "字符串在夹杂数字时，如果数字前后的字符串相同，按数字大小比较。fun 可用函数为 >, >=, <, <=,2020-03-31"
  "T or nil"
  "(string:num-compare < \"a5\"
     \"a13\")"
  (apply (quote or)
    (mapcar (quote (lambda (a b)
          (cond ((and (string:intp a)
                (string:intp b))
              (fun (atoi a)
                (atoi b)))
            ((and (string:realp a)
                (string:realp b))
              (fun (atof a)
                (atof b)))
            (t (fun a b)))))
      (string:auto-split str1)
      (string:auto-split str2))))
```
</details>

---

### string:number-format

**说明**: 格式化数字输出。使输出的字符串长度一致。参数: str-num:数字字符串，int-n,整数位数，int-fraction:小数位数，str-fill:填充字符，字符串第一个为前缀，最后一个为后缀

**用法**:
```lisp
(string:number-format str-num int-n int-fraction str-fill)
```

**参数**: 1 str-num  : 字符串;2 int-n  : 整数;3 int-fraction  : 整数;4 str-fill  : 字符串;

**返回值**: 字符串

**示例**:
```lisp
(string:number-format "5.3"n    3 3 " 0")n  = > "  5.300"
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:number-format (str-num int-n int-fraction str-fill / lst-num part-int part-fraction pre-str post-str)
  "格式化数字输出。使输出的字符串长度一致。参数: str-num:数字字符串，int-n,整数位数，int-fraction:小数位数，str-fill:填充字符，字符串第一个为前缀，最后一个为后缀"
  "字符串"
  "(string:number-format \"5.3\"\n    3 3 \"
    0\")\n  = > \"
   5.300\""
  (if (numberp str-fill)
    (setq str-fill (rtos str-fill 2 0)))
  (setq pre-str (string:l2s-ansi (list (car (string:s2l-ansi str-fill)))))
  (setq post-str (string:l2s-ansi (list (last (string:s2l-ansi str-fill)))))
  (progn (setq lst-num (string:to-list str-num "."))
    (setq part-int (car lst-num))
    (setq part-fraction (cadr lst-num))
    (repeat (- int-n (strlen part-int))
      (setq part-int (strcat pre-str part-int))))
  (if (and part-fraction (> (strlen part-fraction)
        int-fraction))
    (setq part-fraction (cadr (string:to-list (rtos (atof (strcat "0."
                part-fraction))
            2 int-fraction)
          "."))))
  (if part-fraction (repeat (- int-fraction (strlen part-fraction))
      (setq part-fraction (strcat part-fraction post-str)))
    (progn (setq part-fraction "")
      (repeat int-fraction (setq part-fraction (strcat part-fraction post-str)))))
  (if (or (= ""
        part-fraction)
      (null part-fraction))
    (strcat part-int)
    (strcat part-int "."
      part-fraction)))
```
</details>

---

### string:number2chinese

**说明**: 将数字转化为汉语大写.,仅支持到万亿，只支持整数

**用法**:
```lisp
(string:number2chinese num)
```

**参数**: 1 num  : 未识别定义;

**返回值**: String

**示例**:
```lisp
(string:number2chinese 3587823416)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:number2chinese (num / charofcash i n tempa tempstr)
  "将数字转化为汉语大写.,仅支持到万亿，只支持整数"
  "String"
  "(string:number2chinese 3587823416)"
  (defun _n2big (str)
    (setq str (string:subst-all "壹"
        "1"
        str))
    (setq str (string:subst-all "贰"
        "2"
        str))
    (setq str (string:subst-all "叁"
        "3"
        str))
    (setq str (string:subst-all "肆"
        "4"
        str))
    (setq str (string:subst-all "伍"
        "5"
        str))
    (setq str (string:subst-all "陆"
        "6"
        str))
    (setq str (string:subst-all "柒"
        "7"
        str))
    (setq str (string:subst-all "捌"
        "8"
        str))
    (setq str (string:subst-all "玖"
        "9"
        str)))
  (setq num (rtos num 2 0))
  (setq i -1 charofcash "")
  (setq n (strlen num))
  (repeat n (setq i (1+ i))
    (setq tempstr (substr num (- n i)
        1))
    (cond ((= i 0)
        (if (= "零"
            tempstr)
          (setq tempstr "")))
      ((= i 1)
        (if (/= "零"
            tempstr)
          (setq tempstr (strcat tempstr "拾"))))
      ((= i 2)
        (if (/= "零"
            tempstr)
          (setq tempstr (strcat tempstr "百"))))
      ((= i 3)
        (if (/= "零"
            tempstr)
          (setq tempstr (strcat tempstr "千"))))
      ((= i 4)
        (if (= "零"
            tempstr)
          (setq tempstr "万")
          (setq tempstr (strcat tempstr "万"))))
      ((= i 5)
        (if (/= "零"
            tempstr)
          (setq tempstr (strcat tempstr "十"))))
      ((= i 6)
        (if (/= "零"
            tempstr)
          (setq tempstr (strcat tempstr "百"))))
      ((= i 7)
        (if (/= "零"
            tempstr)
          (setq tempstr (strcat tempstr "千"))))
      ((= i 8)
        (if (= "零"
            tempstr)
          (setq tempstr "亿")
          (setq tempstr (strcat tempstr "亿"))))
      ((= i 9)
        (if (/= "零"
            tempstr)
          (setq tempstr (strcat tempstr "十"))))
      ((= i 10)
        (if (/= "零"
            tempstr)
          (setq tempstr (strcat tempstr "百"))))
      ((= i 11)
        (if (/= "零"
            tempstr)
          (setq tempstr (strcat tempstr "千"))))
      ((= i 12)
        (if (/= "零"
            tempstr)
          (setq tempstr (strcat tempstr "万")))))
    (setq tempa (substr charofcash 1 2))
    (setq charofcash (if (= tempstr "零")
        (cond ((= tempa "零")
            charofcash)
          ((= tempa "")
            charofcash)
          ((= tempa "万")
            charofcash)
          ((= tempa "亿")
            charofcash)
          (t (strcat tempstr charofcash)))
        (strcat tempstr charofcash))))
  (setq temp (substr charofcash 1 4))
  (if (= "壹拾"
      temp)
    (substr charofcash 3)
    charofcash)
  (_n2big charofcash))
```
</details>

---

### string:numberp

**说明**: 确定字符串是否为数字

**用法**:
```lisp
(string:numberp str)
```

**参数**: 1 str  : 字符串;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:numberp (str)
  "确定字符串是否为数字"
  (numberp (vl-catch-all-apply (quote read)
      (list (cond ((= "."
              (substr str 1 1))
            (strcat "0"
              str))
          ((and (= "-."
                (substr str 1 2))
              (> (strlen str)
                2))
            (strcat "-0"
              (substr str 2)))
          ((and (= "+."
                (substr str 1 2))
              (> (strlen str)
                2))
            (strcat "+0"
              (substr str 2)))
          (t str))))))
```
</details>

---

### string:parse-by-lst

**说明**: 字符串按分隔符列表转列表

**用法**:
```lisp
(string:parse-by-lst lstr delimlst)
```

**参数**: 1 lstr  : 列表;2 delimlst  : 未明确定义;

**返回值**: 拆分后的列表

**示例**:
```lisp
(string:parse-by-lst "a-b=c" '("-" "="))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:parse-by-lst (lstr delimlst)
  "字符串按分隔符列表转列表"
  "拆分后的列表"
  "(string:parse-by-lst \"a-b=c\"
    '(\"-\"
      \"=\"))"
  (setq lstr (list lstr))
  (foreach del delimlst (setq lstr (apply (quote append)
        (mapcar (quote (lambda (x)
              (string:to-lst x del)))
          lstr))))
  (if (member "
      "
      delimlst)
    (vl-remove ""
      lstr)
    lstr))
```
</details>

---

### string:readme

**说明**: 字符串操作相关函数。

**用法**:
```lisp
(string:readme )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:readme nil "字符串操作相关函数。"
  (princ "字符串操作相关函数。使用 (require 'string:*)
    加载这些函数")
  (princ))
```
</details>

---

### string:realp

**说明**: 确定字符串是否为实数

**用法**:
```lisp
(string:realp str)
```

**参数**: 1 str  : 字符串;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:realp (str)
  "确定字符串是否为实数"
  (and (string:numberp str)
    (or (= "."
        (substr str 1 1))
      (= "-."
        (substr str 1 2))
      (= "+."
        (substr str 1 2))
      (= (quote int)
        (type (read str)))
      (= (quote real)
        (type (read str))))))
```
</details>

---

### string:regexp-replace

**说明**: 正则表达式替换字串

**用法**:
```lisp
(string:regexp-replace string newstr express key)
```

**参数**: 1 string  : 字符串;2 newstr  : 未明确定义;3 express  : 未明确定义;4 key  : 键，关键字;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:regexp-replace (string newstr express key / regex s)
  "正则表达式替换字串"
  (setq regex (vlax-create-object "Vbscript.RegExp"))
  (if (and key (wcmatch key "*g*,*G*"))
    (vlax-put-property regex "Global"
      1)
    (vlax-put-property regex "Global"
      0))
  (if (and key (wcmatch key "*i*,*I*"))
    (vlax-put-property regex "IgnoreCase"
      1)
    (vlax-put-property regex "IgnoreCase"
      0))
  (if (and key (wcmatch key "*m*,*M*"))
    (vlax-put-property regex "Multiline"
      1)
    (vlax-put-property regex "Multiline"
      0))
  (vlax-put-property regex "Pattern"
    express)
  (setq s (vlax-invoke-method regex (quote replace)
      string newstr))
  (vlax-release-object regex)
  s)
```
</details>

---

### string:regexp-search

**说明**: 正则表达式搜索字串. Express = 正则表达式 key = 字母 i I m M g G的组合字串

**用法**:
```lisp
(string:regexp-search string express key)
```

**参数**: 1 string  : 字符串;2 express  : 未明确定义;3 key  : 键，关键字;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:regexp-search (string express key / regex s pos len str l)
  "正则表达式搜索字串. Express = 正则表达式 key = 字母 i I m M g G的组合字串"
  (setq regex (vlax-create-object "Vbscript.RegExp"))
  (if (and key (wcmatch key "*g*,*G*"))
    (vlax-put-property regex "Global"
      1)
    (vlax-put-property regex "Global"
      0))
  (if (and key (wcmatch key "*i*,*I*"))
    (vlax-put-property regex "IgnoreCase"
      1)
    (vlax-put-property regex "IgnoreCase"
      0))
  (if (and key (wcmatch key "*m*,*M*"))
    (vlax-put-property regex "Multiline"
      1)
    (vlax-put-property regex "Multiline"
      0))
  (vlax-put-property regex "Pattern"
    express)
  (setq s (vlax-invoke-method regex (quote execute)
      string))
  (vlax-for o s (setq pos (vlax-get-property o "FirstIndex")
      len (vlax-get-property o "Length")
      str (vlax-get-property o "value"))
    (setq l (cons (list pos len str)
        l)))
  (vlax-release-object regex)
  (reverse l))
```
</details>

---

### string:reverse

**说明**: 反转字符串,支持中文

**用法**:
```lisp
(string:reverse str)
```

**参数**: 1 str  : 字符串;

**返回值**: Str

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:reverse (str)
  "反转字符串,支持中文"
  "Str"
  (string:l2s-ansi (reverse (string:s2l-ansi str))))
```
</details>

---

### string:rightsubstr

**说明**: 从右侧求子串

**用法**:
```lisp
(string:rightsubstr str start len)
```

**参数**: 1 str  : 字符串;2 start  : 未明确定义;3 len  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:rightsubstr (str start len)
  "从右侧求子串"
  (substr str (- (strlen str)
      len start -2)
    len))
```
</details>

---

### string:rsubstr

**说明**: 从右侧求子串

**用法**:
```lisp
(string:rsubstr str len)
```

**参数**: 1 str  : 字符串;2 len  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:rsubstr (str len)
  "从右侧求子串"
  (substr str (- (strlen str)
      len -1)))
```
</details>

---

### string:s2l-ansi

**说明**: 字符串转字byte or word 整数值列表。n当小于128时，单字节，当两个连续的大于128时，双字节值。用于转换非英文字串时防止重码。n当AutoCAD2021且lispsys=1时，返回 unicode 码。

**用法**:
```lisp
(string:s2l-ansi str)
```

**参数**: 1 str  : 字符串;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:s2l-ansi (str / lst-src lst-str h% res)
  "字符串转字byte or word 整数值列表。\n当小于128时，单字节，当两个连续的大于128时，双字节值。用于转换非英文字串时防止重码。\n当AutoCAD2021且lispsys=1时，返回 unicode 码。"
  (if (and (getvar "lispsys")
      (<= 1 (getvar "lispsys")))
    (vl-string->list str)
    (progn (setq h% 0)
      (setq res (quote nil))
      (setq lst-src (vl-string->list str))
      (while (setq byte% (car lst-src))
        (cond ((and (= 0 h%)
              (apply (quote and)
                (mapcar (quote =)
                  (list 92 85 43)
                  lst-src)))
            (setq lst-src (cdr lst-src))
            (setq lst-src (cdr lst-src))
            (setq lst-src (cdr lst-src))
            (setq res (cons (fix (m:hex2dec (strcat "0x"
                      (chr (car lst-src))
                      (chr (cadr lst-src))
                      (chr (nth 2 lst-src))
                      (chr (nth 3 lst-src)))))
                res))
            (setq lst-src (cdr lst-src))
            (setq lst-src (cdr lst-src))
            (setq lst-src (cdr lst-src)))
          ((and (= 0 h%)
              (/= byte% 92)
              (or (< byte% 128)
                (> byte% 255)))
            (setq res (cons byte% res)))
          ((and (= 0 h%)
              (> 256 byte% 127))
            (setq h% byte%))
          ((or (and (> 170 h% 127)
                (< byte% 128))
              (and (> 256 h0 170)
                (> 64 byte%)))
            (setq res (cons h% res))
            (setq res (cons byte% res))
            (setq h% 0))
          ((or (and (> 170 h% 127)
                (> 256 byte% 127))
              (and (> 256 h% 170)
                (> byte% 64)))
            (setq res (cons (+ (lsh h% 8)
                  byte%)
                res))
            (setq h% 0)))
        (setq lst-src (cdr lst-src)))
      (reverse res))))
```
</details>

---

### string:search

**说明**: 搜索字符串pattern 是否为 str的子串，是返回位置（汉字占一位）。

**用法**:
```lisp
(string:search pattern str)
```

**参数**: 1 pattern  : 未识别定义;2 str  : 字符串;

**返回值**: int

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:search (pattern str / lst-pattern lst-str pos-st n)
  "搜索字符串pattern 是否为 str的子串，是返回位置（汉字占一位）。"
  "int"
  (setq lst-pattern (string:s2l-ansi pattern))
  (setq len-pattern (length lst-pattern))
  (setq lst-str (string:s2l-ansi str))
  (if (setq lst-substr (member (car lst-pattern)
        lst-str))
    (progn (setq pos-st (- (length lst-str)
          (length lst-substr)))
      (setq n 1)
      (while (and (cdr lst-pattern)
          (= (car (setq lst-pattern (cdr lst-pattern)))
            (car (setq lst-substr (cdr lst-substr)))))
        (setq n (1+ n)))
      (if (= n len-pattern)
        pos-st))))
```
</details>

---

### string:sort-by-number

**说明**: 按数字排序字符串在夹杂数字时，如果数字前后的字符串相同，按数字大小排序,支持中文数字

**用法**:
```lisp
(string:sort-by-number lst-str)
```

**参数**: 1 lst-str  : 列表;

**返回值**: 排序后的字符串表

**示例**:
```lisp
(string:sort-by-number '("a5"       "a1"      "a8"      "b2"      "b1"      "a110"      "a13"))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:sort-by-number (lst-str)
  "按数字排序字符串\n在夹杂数字时，如果数字前后的字符串相同，按数字大小排序,支持中文数字"
  "排序后的字符串表"
  "(string:sort-by-number '(\"a5\"\n       \"a1\"\n      \"a8\"\n      \"b2\"\n      \"b1\"\n      \"a110\"\n      \"a13\"))"
  (vl-sort lst-str (function (lambda (x y / n a b lx ly)
        (setq n 0)
        (setq lx (string:auto-split x))
        (setq ly (string:auto-split y))
        (while (and (< n (length lx))
            (< n (length ly))
            (= (nth n lx)
              (nth n ly)))
          (setq n (1+ n)))
        (setq a (nth n lx)
          b (nth n ly))
        (if (and a b)
          (cond ((and (string:intp a)
                (string:intp b))
              (< (atoi a)
                (atoi b)))
            ((and (string:realp a)
                (string:realp b))
              (< (atof a)
                (atof b)))
            ((and (string:hannumberp a)
                (string:hannumberp b))
              (< (string:hannumber2number a)
                (string:hannumber2number b)))
            (t (< a b))))))))
```
</details>

---

### string:split

**说明**: 将字符串用separator分隔成列表，separator 可以是字符或由字符组成的表。

**用法**:
```lisp
(string:split str separator)
```

**参数**: 1 str  : 字符串;2 separator  : 分隔符;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:split (str separator / pos)
  "将字符串用separator分隔成列表，separator 可以是字符或由字符组成的表。"
  (cond ((= (quote str)
        (type separator))
      (string:to-list str separator))
    ((listp separator)
      (string:parse-by-lst str separator))))
```
</details>

---

### string:square

**说明**: 字符串自乘

**用法**:
```lisp
(string:square int str)
```

**参数**: 1 int  : 整数;2 str  : 字符串;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:square (int str)
  "字符串自乘"
  (if (zerop int)
    str (strcat str (string:square (1- int)
        str))))
```
</details>

---

### string:subst-all

**说明**: 用 str-new 替换 字符串中所有的 str-old

**用法**:
```lisp
(string:subst-all str-new str-old str)
```

**参数**: 1 str-new  : 字符串;2 str-old  : 字符串;3 str  : 字符串;

**返回值**: 结果字符串

**示例**:
```lisp
(string:subst-all "qwe"    "abc"    "mabcpoildabce")  => "mqwepoildqwee"
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:subst-all (str-new str-old str / inc len)
  "用 str-new 替换 字符串中所有的 str-old "
  "结果字符串"
  "(string:subst-all \"qwe\"\n    \"abc\"\n    \"mabcpoildabce\")\n  => \"mqwepoildqwee\""
  (if (> (strlen str-old)
      0)
    (progn (setq len (strlen str-new)
        inc 0)
      (while (setq inc (vl-string-search str-old str inc))
        (setq str (vl-string-subst str-new str-old str inc)
          inc (+ inc len)))))
  str)
```
</details>

---

### string:substr

**说明**: 截取字符串，ansi 字符按 1 位 计算。start 最小值为 1, len 最小为1

**用法**:
```lisp
(string:substr str start len)
```

**参数**: 1 str  : 字符串;2 start  : 未明确定义;3 len  : 未明确定义;

**返回值**: 结果字符串

**示例**:
```lisp
(string:substr "a中国人bcd开发lisp" 2 3) => 中国人
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:substr (str start len / res i)
  "截取字符串，ansi 字符按 1 位 计算。start 最小值为 1, len 最小为1 "
  "结果字符串"
  "(string:substr \"a中国人bcd开发lisp\"
    2 3)
  => 中国人"
  (setq start (fix start)
    len (fix len))
  (if (< start 1)
    (setq start 1))
  (if (< len 1)
    (setq len 1))
  (setq res nil)
  (setq i -2)
  (repeat len (setq res (cons (nth (+ (setq i (1+ i))
            start)
          (string:s2l-ansi str))
        res)))
  (string:l2s-ansi (vl-remove nil (reverse res))))
```
</details>

---

### string:to-list

**说明**: 字符串转成列表，拆分字符串，指定字符来分隔字符串。

**用法**:
```lisp
(string:to-list str separator)
```

**参数**: 1 str  : 字符串;2 separator  : 分隔符;

**返回值**: 拆分后的列表

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:to-list (str separator / s2l)
  "字符串转成列表，拆分字符串，指定字符来分隔字符串。"
  "拆分后的列表"
  (if (and (p:stringp separator)
      (p:stringp str))
    (if (= ""
        separator)
      (mapcar (quote (lambda (x)
            (string:l2s-ansi (list x))))
        (string:s2l-ansi str))
      (progn (defun s2l (str separator / pos)
          (if (setq pos (vl-string-search separator str))
            (cons (substr str 1 pos)
              (s2l (substr str (+ pos 1 (strlen separator)))
                separator))
            (list str)))
        (s2l str separator)))
    (progn (@:log "ERROR"
        "parameter is not a string")
      nil)))
```
</details>

---

### string:to-lst

**说明**: 字符串转成列表

**用法**:
```lisp
(string:to-lst str separator)
```

**参数**: 1 str  : 字符串;2 separator  : 分隔符;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:to-lst (str separator / pos)
  "字符串转成列表"
  (if (= ""
      separator)
    (mapcar (quote (lambda (x)
          (string:l2s-ansi (list x))))
      (string:s2l-ansi str))
    (if (setq pos (vl-string-search separator str))
      (cons (substr str 1 pos)
        (string:to-lst (substr str (+ pos 1 (strlen separator)))
          separator))
      (list str))))
```
</details>

---

### string:trim-space

**说明**: 去除字符串中的空格

**用法**:
```lisp
(string:trim-space string)
```

**参数**: 1 string  : 字符串;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun string:trim-space (string)
  "去除字符串中的空格"
  (string:subst-all ""
    "
    "
    string))
```
</details>

---

## style

**模块说明**: 专用功能模块

### style:missing-fonts

**说明**: 检查缺少的字体，如果有返回字体文件名组成的列表，没有返回nil

**用法**:
```lisp
(style:missing-fonts )
```

**参数**: None

**返回值**: 字体文件名组成的列表,或nil

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun style:missing-fonts (/ lst-missing st)
  "检查缺少的字体，如果有返回字体文件名组成的列表，没有返回nil"
  "字体文件名组成的列表,或nil"
  ""
  (setq st (tblnext "style"
      t))
  (setq lst-missing (cons (list (cdr (assoc 3 st))
        (cdr (assoc 4 st)))
      nil))
  (while (setq st (tblnext "style"))
    (setq lst-missing (cons (list (cdr (assoc 3 st))
          (cdr (assoc 4 st)))
        lst-missing)))
  (vl-remove nil (mapcar (quote (lambda (x)
          (cond ((null (vl-filename-extension x))
              (if (findfile (strcat x ".shx"))
                nil x))
            ((member (strcase (vl-filename-extension x)
                  t)
                (quote (".shx")))
              (if (findfile x)
                nil x))
            ((member (strcase (vl-filename-extension x)
                  t)
                (quote (".ttf"
                    ".ttc")))
              (if (findfile (strcat (getenv "windir")
                    "\\Fonts\\"
                    X))
                nil X)))))
      (LIST:REMOVE-DUPLICATES (VL-REMOVE nil (VL-REMOVE ""
            (APPLY (QUOTE APPEND)
              LST-MISSING)))))))
```
</details>

---

## svg

**模块说明**: 专用功能模块

### svg:export

**说明**: 将选择的图元生成SVG, ss 选择集,path 输出文件路径。当前只支持二维多段线和线段

**用法**:
```lisp
(svg:export ss path)
```

**参数**: 1 ss  : 选择集;2 path  : 文件路径;

**示例**:
```lisp
(svg:export (ssget) "C:/example.svg")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun svg:export (ss path / i out lst-ss color w h ox oy)
  "将选择的图元生成SVG, ss 选择集,path 输出文件路径。当前只支持二维多段线和线段"
  ""
  "(svg:export (ssget)
    \"C:/example.svg\")"
  (setq lst-ss (pickset:to-list ss))
  (setq box (pickset:getbox ss 1))
  (setq w (- (car (cadr box))
      (car (car box))))
  (setq h (- (cadr (cadr box))
      (cadr (car box))))
  (setq ox (car (car box)))
  (setq oy (cadr (car box)))
  (setq out (open path "w"))
  (write-line "<?xml version=\"1.0\"
    encoding=\"utf-8\"
    standalone=\"no\"?>"
    out)
  (write-line (strcat "<svg width=\"256\"
      height=\"256\"
      viewBox=\"0 0 "
      (rtos w 2 1)
      "
      "
      (rtos h 2 1)
      "\"
      version=\"1.1\"
      xmlns=\"http://www.w3.org/2000/svg\"
      xmlns:xlink=\"http://www.w3.org/1999/xlink\">")
    out)
  (foreach curve lst-ss (setq color (color:rgb2css (entity:get-truecolor curve)))
    (setq bold (entity:getdxf curve 43))
    (if (or (null bold)
        (and bold (= 0 bold)))
      (setq bold 1))
    (cond ((wcmatch (entity:getdxf curve 0)
          "LWPOLYLINE,LINE")
        (write-line (strcat "<polyline points=\""
            (string:from-list (mapcar (quote (lambda (x)
                    (strcat (rtos (- (car x)
                          ox)
                        2 3)
                      ","
                      (rtos (- h (- (cadr x)
                            oy))
                        2 3))))
                (progn (setq pts (curve:get-points curve))
                  (if (and (setq closed (entity:getdxf curve 70))
                      (= 1 closed))
                    (setq pts (append pts (list (car pts))))
                    pts)))
              "
              ")
            "\"
            style=\"fill:none;stroke:"
            color ";stroke-width:"
            (rtos bold 2 3)
            "\"
            />")
          out))
      ((wcmatch (entity:getdxf curve 0)
          "CIRCLE")
        (setq x (entity:getdxf curve 10))
        (write-line (strcat "<circle cx=\""
            (rtos (- (car x)
                ox)
              2 3)
            "\"
            "
            "cy=\""
            (rtos (- h (- (cadr x)
                  oy))
              2 3)
            "\"
            "
            "r=\""
            (rtos (entity:getdxf curve 40)
              2 3)
            "\"
            "
            "stroke=\""
            color "\"
            "
            "stroke-width=\""
            (rtos bold 2 3)
            "\"
            "
            "fill=\"white\"
            />")
          out))))
  (write-line "</svg>"
    out)
  (close out)
  (princ))
```
</details>

---

## sys

**模块说明**: 专用功能模块

### sys:list-process

**说明**: 获取当前运行的进程对象列表。

**用法**:
```lisp
(sys:list-process )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun sys:list-process (/ vlist vobj lcom lexecquery item)
  "获取当前运行的进程对象列表。"
  ""
  (vl-load-com)
  (setq vlist (quote nil))
  (if (setq vobj (vlax-create-object "wbemscripting.swbemlocator"))
    (progn (setq lcom (vlax-invoke vobj (quote connectserver)
          "."
          "\\root\\cimv2"
          ""
          ""
          ""
          ""
          128 nil))
      (setq lexecquery (vlax-invoke lcom (quote execquery)
          "Select * from Win32_Process"))
      (vlax-for item lexecquery (setq vlist (cons item vlist)))
      (vlax-release-object lexecquery)
      (vlax-release-object lcom)
      (vlax-release-object vobj)))
  vlist)
```
</details>

---

### sys:list-process-name

**说明**: 获取当前运行的进程名(exe文件名)。

**用法**:
```lisp
(sys:list-process-name )
```

**参数**: None

**返回值**: String

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun sys:list-process-name nil "获取当前运行的进程名(exe文件名)。"
  "String"
  (mapcar (quote (lambda (x)
        (vlax-get x (quote name))))
    (sys:list-process)))
```
</details>

---

## system

**模块说明**: 系统信息、环境变量

### system:dir

**说明**: 字符串路径目录化，即结尾是 \

**用法**:
```lisp
(system:dir str)
```

**参数**: 1 str  : 字符串;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun system:dir (str)
  "字符串路径目录化，即结尾是 \\ "
  (if (= 92 (last (vl-string->list str)))
    str (strcat str "\\")))
```
</details>

---

### system:explorer

**说明**: 在资源管理器中打开指定的文件夹

**用法**:
```lisp
(system:explorer dir-path)
```

**参数**: 1 dir-path  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun system:explorer (dir-path)
  "在资源管理器中打开指定的文件夹"
  (startapp (strcat "explorer /e,\""
      (@::path-os dir-path)
      "\""))
  (princ))
```
</details>

---

### system:get-folder

**说明**: 调用Windows通用目录选取对话框,返回选中路径.参数: msg-对话框提示字符串

**用法**:
```lisp
(system:get-folder msg)
```

**参数**: 1 msg  : 提示信息;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun system:get-folder (msg / winshell shfolder path catchit)
  "调用Windows通用目录选取对话框,返回选中路径.\n参数: msg-对话框提示字符串"
  (vl-load-com)
  (setq winshell (vlax-create-object "Shell.Application"))
  (setq shfolder (vlax-invoke-method winshell (quote browseforfolder)
      0 msg 1))
  (setq catchit (vl-catch-all-apply (quote (lambda nil (setq shfolder (vlax-get-property shfolder (quote self)))
          (setq path (vlax-get-property shfolder (quote path)))))))
  (if (vl-catch-all-error-p catchit)
    nil path))
```
</details>

---

### system:get-qqids

**说明**: 获取本机登录过的QQ号

**用法**:
```lisp
(system:get-qqids )
```

**参数**: 没有一个

**返回值**: list

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun system:get-qqids nil "获取本机登录过的QQ号"
  "list"
  (setq u (getenv "userprofile"))
  (if (vl-file-directory-p (strcat u "\\Documents\\Tencent Files"))
    (progn (setq qqids (vl-directory-files (strcat u "\\Documents\\Tencent Files")
          "*"
          -1))
      (setq qqids (vl-remove-if-not (quote (lambda (x)
              (wcmatch x "####*")))
          qqids))))
  qqids)
```
</details>

---

### system:platform

**说明**: 返回windows操作系统版本信息

**用法**:
```lisp
(system:platform )
```

**参数**: 没有一个

**返回值**: lst

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun system:platform nil "返回windows操作系统版本信息"
  "lst"
  (mapcar (quote (lambda (x)
        (cons x (vl-registry-read "HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion"
            x))))
    (list "CurrentVersion"
      "ProductName"
      "EditionId"
      "DisplayVersion"
      "CurrentBuild")))
```
</details>

---

### system:python

**说明**: 安装python

**用法**:
```lisp
(system:python )
```

**参数**: 没有一个

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun system:python nil "安装python"
  (if (and (null (findfile (@::path-os (strcat (getenv "userprofile")
              "/AppData/Local/Programs/Python/Python313/python.EXE"))))
      (system:winget))
    (@::cmd "start"
      "winget install python.python.3.13")))
```
</details>

---

### system:vers

**说明**: CAD版本号

**用法**:
```lisp
(system:vers )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun system:vers nil "CAD版本号"
  (quote ((autocad (2000 . 15.0)
        (2000i . 15.1)
        (2002 . 15.2)
        (2004 . 16.0)
        (2005 . 16.1)
        (2006 . 16.2)
        (2007 . 17.0)
        (2008 . 17.1)
        (2009 . 17.2)
        (2010 . 18.0)
        (2011 . 18.1)
        (2012 . 18.2)
        (2013 . 19.0)
        (2014 . 19.1)
        (2015 . 20.0)
        (2016 . 20.1)
        (2017 . 21.0)
        (2018 . 22.0)
        (2019 . 23.0)
        (2020 . 23.1)
        (2021 . 24.0)
        (2022 . 24.1))
      (gstarcad)
      (zwcad))))
```
</details>

---

### system:which

**说明**: 查找定位可执行文件

**用法**:
```lisp
(system:which exename)
```

**参数**: 1 exename  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun system:which (exename / res logfile)
  "查找定位可执行文件"
  (progn (setq logfile (@::path-os (strcat @::*tmp-path* "pslog")))
    (@::cmd "powershell"
      (strcat "Get-Command "
        exename "
        -errorAction SilentlyContinue | Out-File -FilePath "
        logfile "}"))))
```
</details>

---

### system:winget

**说明**: 早期的win10没有winget,这个函数用于安装winget

**用法**:
```lisp
(system:winget )
```

**参数**: 没有一个

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun system:winget nil "早期的win10没有winget,这个函数用于安装winget"
  (setq ps1 (list "if (-not ( Get-Command winget -errorAction SilentlyContinue))
      {"
      "
         Add-AppxPackage -RegisterByFamilyName -MainPackage Microsoft.DesktopAppInstaller_8wekyb3d8bbwe"
      "}"
      "if (-not ( Get-Command winget -errorAction SilentlyContinue))
      {"
      "
         Invoke-WebRequest http://s3.atlisp.cn/download/Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle"
      "
         Add-AppxPackage -RegisterByFamilyName -MainPackage .Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle"
      "}"))
  (setq fp (open (setq file (@::path-os (strcat @::*tmp-path* "get-winget.ps1")))
      "w"))
  (foreach ln ps1 (write-line ln fp))
  (close fp)
  (or @::enable-start (@::check-pgp)
    (@::patch-pgp))
  (@::cmd "powershell"
    (strcat "Get-Content "
      file "| Invoke-Expression"))
  t)
```
</details>

---

### system:winget-src

**说明**: 当uri为 nil时重置为官方源.

**用法**:
```lisp
(system:winget-src uri)
```

**参数**: 1 uri  : 未识别定义;

**示例**:
```lisp
(system:winget-src  "https://mirrors.ustc.edu.cn/winget-source")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun system:winget-src (uri)
  "当uri为 nil时重置为官方源."
  ""
  "(system:winget-src  \"https://mirrors.ustc.edu.cn/winget-source\")"
  (if (not (string-equal "administrator"
        (getenv "username")))
    (@::prompt "本函数需要管理员权限"))
  (or @::enable-start (@::check-pgp)
    (@::patch-pgp))
  (cond ((p:stringp uri)
      (@::cmd "powershell"
        "winget source remove winget;winget source add winget "
        uri))
    ((null uri)
      (@::cmd "powershell"
        "winget source reset winget"))
    (t (@::cmd "powershell"
        "winget source remove winget;winget source add winget \"https://mirrors.ustc.edu.cn/winget-source\""))))
```
</details>

---

### system:wscript

**说明**: 调用WScript 脚本，并返回输出字符串

**用法**:
```lisp
(system:wscript wscode)
```

**参数**: 1 wscode  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun system:wscript (wscode / wsh txt)
  "调用WScript 脚本，并返回输出字符串"
  (setq wsh (vlax-create-object "wscript.shell"))
  (setq txt (vlax-invoke (vlax-get (vlax-invoke wsh (quote exec)
          wscode)
        (quote stdout))
      (quote readall)))
  (vlax-release-object wsh)
  txt)
```
</details>

---

## table

**模块说明**: 专用功能模块

### table:make

**说明**: 创建表格，参数: npt:位置点，ntitle:标题 ， nheaders:表头表nmat-data: 单元数据矩阵,目前仅支持文字型表格

**用法**:
```lisp
(table:make pt title headers mat-data)
```

**参数**: 1 pt  : 单个2D/3D坐标点;2 title  : 未识别定义;3 headers  : 未识别定义;4 mat-data  : 未识别定义;

**返回值**: 表格图元

**示例**:
```lisp
(table:make (getpoint)n    "我的表格"n    '("列1"n      "列2"n      "列3")'((5 3 3)(2 3 3)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun table:make (pt title headers mat-data / unit-h unit-w list-dxf i% rows columns)
  "创建表格，参数: \npt:位置点，\ntitle:标题 ， \nheaders:表头表\nmat-data: 单元数据矩阵,目前仅支持文字型表格"
  "表格图元"
  "(table:make (getpoint)\n    \"我的表格\"\n    '(\"列1\"\n      \"列2\"\n      \"列3\")'((5 3 3)(2 3 3)))"
  (if mat-data (progn (setq rows (+ 2 (length mat-data))
        columns (apply (quote max)
          (mapcar (quote length)
            mat-data)))
      (if (null headers)
        (progn (setq i% 0)
          (while (< i% columns)
            (setq headers (cons (strcat "C"
                  (itoa (- columns i%)))
                headers))
            (setq i% (1+ i%)))))
      (setq unit-h (@:scale 3.5)
        unit-w (@:scale 100.0))
      (setq rows (+ 2 (length mat-data))
        columns (apply (quote max)
          (mapcar (quote length)
            mat-data)))
      (setq list-dxf (list (quote (0 . "ACAD_TABLE"))
          (quote (100 . "AcDbEntity"))
          (quote (67 . 0))
          (quote (100 . "AcDbBlockReference"))
          (cons 10 pt)
          (quote (41 . 1.0))
          (quote (42 . 1.0))
          (quote (43 . 1.0))
          (quote (50 . 0.0))
          (quote (70 . 0))
          (quote (71 . 0))
          (quote (44 . 0.0))
          (quote (45 . 0.0))
          (quote (210 0.0 0.0 1.0))
          (quote (100 . "AcDbTable"))
          (quote (280 . 0))
          (quote (11 1.0 0.0 0.0))
          (quote (90 . 22))
          (cons 91 rows)
          (cons 92 columns)
          (quote (93 . 0))
          (quote (94 . 0))
          (quote (95 . 0))
          (quote (96 . 0))))
      (setq list-dxf (append list-dxf (list (cons 141 (* 1.2 unit-h)))))
      (setq i% 0)
      (while (< i% (1- rows))
        (setq list-dxf (append list-dxf (list (cons 141 unit-h))))
        (setq i% (1+ i%)))
      (setq i% 0)
      (while (< i% columns)
        (setq unit-w (* (@:scale 5.0)
            (apply (quote max)
              (mapcar (quote (lambda (x)
                    (strlen (@:to-string (if (nth i% x)
                          (nth i% x)
                          "")))))
                (cons headers mat-data)))))
        (setq list-dxf (append list-dxf (list (cons 142 unit-w))))
        (setq i% (1+ i%)))
      (setq list-dxf (append list-dxf (list (quote (171 . 1))
            (quote (172 . 0))
            (quote (173 . 0))
            (quote (174 . 0))
            (cons 175 columns)
            (quote (176 . 1))
            (quote (91 . 262144))
            (quote (178 . 0))
            (quote (145 . 0.0))
            (cons 140 (@:scale 2.5))
            (quote (92 . 0))
            (quote (301 . "CELL_VALUE"))
            (quote (93 . 6))
            (quote (90 . 4))
            (cons 1 title)
            (quote (94 . 0))
            (cons 300 "")
            (cons 302 title)
            (quote (304 . "ACVALUE_END")))))
      (setq i% 0)
      (while (< i% (1- columns))
        (setq list-dxf (append list-dxf (list (quote (171 . 1))
              (quote (172 . 0))
              (quote (173 . 1))
              (quote (174 . 0))
              (quote (175 . 1))
              (quote (176 . 1))
              (quote (91 . 0))
              (quote (178 . 0))
              (quote (145 . 0.0))
              (cons 140 (@:scale 2.5))
              (quote (92 . 0))
              (quote (301 . "CELL_VALUE"))
              (quote (93 . 6))
              (quote (90 . 0))
              (quote (94 . 0))
              (cons 300 "")
              (cons 302 "")
              (quote (304 . "ACVALUE_END")))))
        (setq i% (1+ i%)))
      (if headers (progn (foreach header% headers (setq list-dxf (append list-dxf (list (quote (171 . 1))
                  (quote (172 . 0))
                  (quote (173 . 0))
                  (quote (174 . 0))
                  (quote (175 . 1))
                  (quote (176 . 1))
                  (quote (91 . 262144))
                  (quote (178 . 0))
                  (quote (145 . 0.0))
                  (cons 140 (@:scale 2.5))
                  (quote (92 . 0))
                  (quote (301 . "CELL_VALUE"))
                  (quote (93 . 6))
                  (quote (90 . 4))
                  (cons 1 (@:to-string header%))
                  (quote (94 . 0))
                  (cons 300 "")
                  (cons 302 (@:to-string header%))
                  (quote (304 . "ACVALUE_END"))))))
          (if (< (length headers)
              columns)
            (setq i% 0)
            (while (< i% (- columns (length headers)))
              (setq list-dxf (append list-dxf (list (quote (171 . 1))
                    (quote (172 . 0))
                    (quote (173 . 0))
                    (quote (174 . 0))
                    (quote (175 . 1))
                    (quote (176 . 1))
                    (quote (91 . 262144))
                    (quote (178 . 0))
                    (quote (145 . 0.0))
                    (cons 140 (@:scale 2.5))
                    (quote (92 . 0))
                    (quote (301 . "CELL_VALUE"))
                    (quote (93 . 6))
                    (quote (90 . 4))
                    (cons 1 "")
                    (quote (94 . 0))
                    (cons 300 "")
                    (cons 302 "")
                    (quote (304 . "ACVALUE_END")))))
              (setq i% (1+ i%))))))
      (foreach row% mat-data (foreach column% row% (setq list-dxf (append list-dxf (list (quote (171 . 1))
                (quote (172 . 0))
                (quote (173 . 0))
                (quote (174 . 0))
                (quote (175 . 1))
                (quote (176 . 1))
                (quote (91 . 262144))
                (quote (178 . 0))
                (quote (145 . 0.0))
                (cons 140 (@:scale 2.5))
                (quote (92 . 0))
                (quote (301 . "CELL_VALUE"))
                (quote (93 . 6))
                (quote (90 . 4))
                (cons 1 (@:to-string column%))
                (quote (94 . 0))
                (cons 300 "")
                (cons 302 (@:to-string column%))
                (quote (304 . "ACVALUE_END"))))))
        (if (< (length row%)
            columns)
          (progn (setq i% 0)
            (while (< i% (- columns (length row%)))
              (setq list-dxf (append list-dxf (list (quote (171 . 1))
                    (quote (172 . 0))
                    (quote (173 . 0))
                    (quote (174 . 0))
                    (quote (175 . 1))
                    (quote (176 . 1))
                    (quote (91 . 262144))
                    (quote (178 . 0))
                    (quote (145 . 0.0))
                    (cons 140 (@:scale 2.5))
                    (quote (92 . 0))
                    (quote (301 . "CELL_VALUE"))
                    (quote (93 . 6))
                    (quote (90 . 4))
                    (cons 1 "")
                    (quote (94 . 0))
                    (cons 300 "")
                    (cons 302 "")
                    (quote (304 . "ACVALUE_END")))))
              (setq i% (1+ i%))))))
      (setq ent (entmakex list-dxf))
      (vla-settextheight (e2o ent)
        acdatarow (@:scale 2.5))
      (vla-settextheight (e2o ent)
        acheaderrow (@:scale 2.5))
      (vla-settextheight (e2o ent)
        actitlerow (@:scale 3.5))
      (vla-setalignment (e2o ent)
        acdatarow acmiddlecenter)
      ent)))
```
</details>

---

### table:read-csv

**说明**: 读取 csv 文件。

**用法**:
```lisp
(table:read-csv file)
```

**参数**: 1 file  : 文件名;

**返回值**: 二维表

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun table:read-csv (file)
  "读取 csv 文件。"
  "二维表"
  (vl-remove-if (quote (lambda (x)
        (and (= 1 (length x))
          (= ""
            (car x)))))
    (mapcar (quote (lambda (x)
          (string:to-list x ",")))
      (string:to-list (@:get-file-contents (findfile file))
        "\n"))))
```
</details>

---

### table:write-csv

**说明**: 将 二维表 写入 csv 文件。

**用法**:
```lisp
(table:write-csv lst file)
```

**参数**: 1 lst  : 列表;2 file  : 文件名;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun table:write-csv (lst file / fp *error*)
  "将 二维表 写入 csv 文件。"
  ""
  (defun *error* (msg)
    (if (= (quote file)
        (type fp))
      (close fp))
    (@:*error* msg))
  (setq fp (open file "w"))
  (foreach str-line (mapcar (quote (lambda (x)
          (string:from-list (mapcar (quote @:to-string)
              x)
            ",")))
      lst)
    (write-line str-line fp))
  (close fp)
  t)
```
</details>

---

## tbl

**模块说明**: 专用功能模块

### tbl:dxf2obj

**说明**: 将 tbl 名称转化为 ActiveX 对象 Document 的属性名(指向对象集的).

**用法**:
```lisp
(tbl:dxf2obj tbl-name)
```

**参数**: 1 tbl-name  : 未识别定义;

**返回值**: symbol

**示例**:
```lisp
(tbl:dxf2obj "layer")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun tbl:dxf2obj (tbl-name)
  "将 tbl 名称转化为 ActiveX 对象 Document 的属性名(指向对象集的)."
  "symbol"
  "(tbl:dxf2obj \"layer\")"
  (read (cond ((string-equal tbl-name "APPID")
        "RegisteredApplications")
      ((string-equal tbl-name "LTYPE")
        "linetypes")
      ((string-equal tbl-name "STYLE")
        "textstyles")
      ((string-equal tbl-name "UCS")
        "UserCoordinateSystems")
      ((string-equal tbl-name "VPORT")
        "Viewports")
      (t (strcat tbl-name "s")))))
```
</details>

---

### tbl:list

**说明**: 列 DXF表格tbl中的项目的名称。表格为:block,dimstyle,layer,linetype,textstyle,ucs,view,vport.也支持另类的layout，layout 本质是属性 block中的匿名块 *Space# 系列其还有类似 tbl 的如 Materials, tablestyles,mleaderstyle,groups,dictionaries.

**用法**:
```lisp
(tbl:list tbl-name)
```

**参数**: 1 tbl-name  : 未识别定义;

**返回值**: lst

**示例**:
```lisp
(tbl:list "layout") ; => ("Model" "布局1" "布局2")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun tbl:list (tbl-name / tbl)
  "列 DXF表格tbl中的项目的名称。\n表格为:block,dimstyle,layer,linetype,textstyle,ucs,view,vport.\n也支持另类的layout，layout 本质是属性 block中的匿名块 *Space# 系列\n其还有类似 tbl 的如 Materials, tablestyles,mleaderstyle,groups,dictionaries."
  "lst"
  "(tbl:list \"layout\")
  ; => (\"Model\"
    \"布局1\"
    \"布局2\")"
  (setq tbl nil)
  (vlax-for item (vlax-get-property *doc* (tbl:dxf2obj tbl-name))
    (setq tbl (cons (vla-get-name item)
        tbl)))
  (reverse tbl))
```
</details>

---

### tbl:rename

**说明**: 重命名DXF表格tbl中的项目的名称。 表格为:appid,block,dimstyle,layer,ltype,style,ucs,view,vport.注意 layout 不属于 tbl 的内容，但属性对象集合。请使用 layout:rename 函数。参数:tbl-name tbl表格名, old-name 原名称，new-name 新名称

**用法**:
```lisp
(tbl:rename tbl-name old-name new-name)
```

**参数**: 1 tbl-name  : 未识别定义;2 old-name  : 未识别定义;3 new-name  : 未识别定义;

**返回值**: T 成功，nil 失败

**示例**:
```lisp
(tbl:rename "layout" "布局1" "我的布局")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun tbl:rename (tbl-name old-name new-name)
  "重命名DXF表格tbl中的项目的名称。 表格为:appid,block,dimstyle,layer,ltype,style,ucs,view,vport.\n注意 layout 不属于 tbl 的内容，但属性对象集合。请使用 layout:rename 函数。\n参数:tbl-name tbl表格名, old-name 原名称，new-name 新名称"
  "T 成功，nil 失败"
  "(tbl:rename \"layout\"
    \"布局1\"
    \"我的布局\")"
  (if (and (or (string-equal tbl-name "layout")
        (and (tblsearch tbl-name old-name)
          (not (tblsearch tbl-name new-name))))
      (snvalid new-name))
    (progn (if (vl-catch-all-error-p (vl-catch-all-apply (quote vla-put-name)
            (list (vla-item (vlax-get-property *doc* (tbl:dxf2obj tbl-name))
                old-name)
              new-name)))
        nil t))))
```
</details>

---

## text

**模块说明**: 专用功能模块

### text:box

**说明**: 获取单行文本的文本框

**用法**:
```lisp
(text:box ent-text)
```

**参数**: 1 ent-text  : 单个图元;

**返回值**: 4点矩形框

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun text:box (ent-text / tbox box mrz mt)
  "获取单行文本的文本框"
  "4点矩形框"
  (setq tbox (apply (quote point:rec-2pt->4pt)
      (textbox (entget ent-text))))
  (setq r1 (- (entity:getdxf ent-text 50)))
  (setq pt-base (entity:getdxf ent-text 10))
  (setq mrz (list (list (setq cr1 (cos r1))
        (setq sr1 (sin r1))
        0 0)
      (list (- sr1)
        cr1 0 0)
      (list 0 0 1 0)
      (list 0 0 0 1)))
  (setq mt (list (list 1 0 0 (car pt-base))
      (list 0 1 0 (cadr pt-base))
      (list 0 0 1 (caddr pt-base))
      (list 0 0 0 1)))
  (mapcar (quote (lambda (x)
        (matrix:mxp mt (matrix:mxp mrz x))))
    tbox))
```
</details>

---

### text:from-markdown

**说明**: 将 markdown 格式的文本转化为mtext格式

**用法**:
```lisp
(text:from-markdown str)
```

**参数**: 1 str  : 字符串;

**返回值**: String

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun text:from-markdown (str / lst-str str-tmp)
  "将 markdown 格式的文本转化为mtext格式"
  "String"
  (setq lst-str (string:to-list str "\n"))
  (defun handle-strong (str / tmp i res)
    (setq tmp (string:to-list str "**"))
    (setq i 0)
    (foreach unit% tmp (if (= 1 (rem i 2))
        (setq res (cons (strcat "{\\fSimHei|b0|i0|c134|p49;"
              unit% "}")
            res))
        (setq res (cons unit% res)))
      (setq i (1+ i)))
    (string:from-lst (reverse res)
      ""))
  (defun handle-header (str)
    (cond ((= "* "
          (substr str 1 2))
        (strcat "\\C2;· \\C3;"
          (substr str 3)))
      ((= "** "
          (substr str 1 3))
        (strcat "\\C2;·· \\C3;"
          (substr str 4)))
      ((= "*** "
          (substr str 1 4))
        (strcat "\\C2;··· \\C3;"
          (substr str 5)))
      ((= "**** "
          (substr str 1 5))
        (strcat "\\C2;···· \\C3;"
          (substr str 6)))
      (t str)))
  (setq lst-str (mapcar (quote handle-header)
      lst-str))
  (setq lst-str (mapcar (quote handle-strong)
      lst-str))
  (string:from-list lst-str "\\P"))
```
</details>

---

### text:get-matrix

**说明**: 从dwg图中框取单行文本，形成二维列表数据

**用法**:
```lisp
(text:get-matrix )
```

**参数**: None

**返回值**: 由字符串组成的二维列表

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun text:get-matrix (/ ss-txt result lst-tmp row)
  "从dwg图中框取单行文本，形成二维列表数据"
  "由字符串组成的二维列表"
  ""
  (setq ss-txt (pickset:to-list (ssget (quote ((0 . "text"))))))
  (setq ss-txt (vl-sort ss-txt (quote (lambda (x y)
          (or (and (equal (cadr (entity:getdxf x 10))
                (cadr (entity:getdxf y 10))
                (* 1.1 (entity:getdxf x 40)))
              (< (car (entity:getdxf x 10))
                (car (entity:getdxf y 10))))
            (> (cadr (entity:getdxf x 10))
              (+ (cadr (entity:getdxf y 10))
                (* 0.5 (entity:getdxf y 40)))))))))
  (setq result (quote nil))
  (setq lst-tmp (list (car ss-txt)))
  (while (setq ss-txt (cdr ss-txt))
    (if (equal (cadr (entity:getdxf (last lst-tmp)
            10))
        (cadr (entity:getdxf (car ss-txt)
            10))
        (* 0.3 (entity:getdxf (last lst-tmp)
            40)))
      (setq lst-tmp (append lst-tmp (list (car ss-txt))))
      (progn (setq result (append result (list lst-tmp)))
        (setq lst-tmp (list (car ss-txt))))))
  (setq result (append result (list lst-tmp)))
  (setq lst-tmp (cdr result))
  (setq result (list (car result)))
  (foreach row lst-tmp (if (= (length row)
        (length (car result)))
      (setq result (append result (list row)))
      (progn (setq i 0)
        (setq j 0)
        (setq row-tmp (quote nil))
        (repeat (length (car result))
          (if (equal (car (entity:getdxf (nth j row)
                  10))
              (car (entity:getdxf (nth i (car result))
                  10))
              (* 1.1 (entity:getdxf (nth i (car result))
                  40)))
            (progn (setq row-tmp (append row-tmp (list (nth j row))))
              (setq i (1+ i))
              (setq j (1+ j)))
            (progn (setq row-tmp (append row-tmp (list "")))
              (setq i (1+ i)))))
        (setq result (append result (list row-tmp))))))
  (mapcar (quote (lambda (x)
        (mapcar (quote (lambda (y)
              (if (= (quote ename)
                  (type y))
                (entity:getdxf y 1)
                y)))
          x)))
    result))
```
</details>

---

### text:get-mtext

**说明**: 取多行文件的内容。当内容长度大于255时，会有多个组码 3.本函数用于组合这些字符串为一整体。

**用法**:
```lisp
(text:get-mtext en-mtext)
```

**参数**: 1 en-mtext  : 单个图元;

**返回值**: 字符串

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun text:get-mtext (en-mtext / dxfs)
  "取多行文件的内容。当内容长度大于255时，会有多个组码 3.本函数用于组合这些字符串为一整体。"
  "字符串"
  ""
  (setq dxfs (entget en-mtext))
  (apply (quote strcat)
    (mapcar (quote cdr)
      (vl-remove-if (quote (lambda (x)
            (and (/= 3 (car x))
              (/= 1 (car x)))))
        dxfs))))
```
</details>

---

### text:gettextwidth

**用法**:
```lisp
(text:gettextwidth str edata)
```

**参数**: 1 str  : 字符串;  2 edata  : 未明确定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun text:gettextwidth (str edata / box1 bo2)
    (if (not str)
        (setq str (cdr (assoc 1 edata))))
    (setq box1 (textbox (list-substassoc (list (cons 1 (strcat "m"
                            str "m")))
                edata t)))
    (setq box2 (textbox (list-substassoc (list (cons 1 "mm"))
                edata t)))
    (- (- (caadr box1)
            (caar box1))
        (- (caadr box2)
            (caar box2))))
```
</details>

---

### text:mtext->text

**说明**: 去除多行文本中的格式化字符串，本函数为测试版，不保证结果正确。

**用法**:
```lisp
(text:mtext->text str-m)
```

**参数**: 1 str-m  : 字符串;

**返回值**: 结果字符串

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun text:mtext->text (str-m / lst-str str-m% purge-fmt)
    "去除多行文本中的格式化字符串，本函数为测试版，不保证结果正确。"
    "结果字符串"
    (if (and (= "{"
                (substr str-m 1 1))
            (= "}"
                (substr str-m (strlen str-m)
                    1)))
        (@:list-to-string (mapcar (function (lambda (x)
                        (@:string-subst "
                            "
                            "\\p"
                            (if (or (/= "\\"
                                        (substr x 1 1))
                                    (= "\\p"
                                        (substr x 1 2)))
                                x ""))))
                (@:string-to-list (substr str-m 2 (- (strlen str-m)
                            2))
                    ";"))
            "")
        str-m))
```
</details>

---

### text:mtext->text2

**说明**: 去除多行文本中的格式化字符串，本函数为测试版，不保证结果正确。

**用法**:
```lisp
(text:mtext->text2 str-m)
```

**参数**: 1 str-m  : 字符串;

**返回值**: 结果字符串

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun text:mtext->text2 (str-m / lst-str str-m% purge-fmt)
    "去除多行文本中的格式化字符串，本函数为测试版，不保证结果正确。"
    "结果字符串"
    (setq lst-str (string:parse-by-lst str-m (quote ("\\"
                    "{"
                    "}"))))
    (defun purge-fmt (str-m)
        (if (/= ""
                str-m)
            (cond ((and (/= "s"
                            (substr str-m 1 1))
                        (= ";"
                            (substr str-m (strlen str-m))))
                    (setq str-m ""))
                ((and (= "s"
                            (substr str-m 1 1))
                        (= ";"
                            (substr str-m (strlen str-m)))
                        (setq str-m (substr str-m 2 (- (strlen str-m)
                                    2)))))
                (t (setq str-m (substr str-m 2)))))
        (setq lst-str (string:to-list str-m ";"))
        (if (> (length lst-str)
                1)
            (setq str-m (last lst-str)))
        str-m)
    (string:from-list (vl-remove ""
            (mapcar (quote purge-fmt)
                lst-str))
        ""))
```
</details>

---

### text:parse-mtext

**说明**: 解析多行文本中的格式化字符串，目前为测试版，不保证结果完全正确。

**用法**:
```lisp
(text:parse-mtext str-m)
```

**参数**: 1 str-m  : 字符串;

**返回值**: 结果表. ((fmt . t)(str)...(fmt . t)). 某单元为点对且 (cdr 单元) 为 t 时表示为格式串，nil 时为的内容串

**示例**:
```lisp
(text:parse-mtext "\\A1;m{\\H0.7x;\\S3^;} {\\H0.7x;\\S^2;\\H1.4286x; aN\\KN BBB\\k  \\OOO \\oabc  \\P\\P\\fSimSun|b0|i0|c134|p2;sdfaf}")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun text:parse-mtext (str-m / flag flag-char str-list lst-res fmt% str%)
  "解析多行文本中的格式化字符串，目前为测试版，不保证结果完全正确。"
  "结果表. ((fmt . t)(str)...(fmt . t)). 某单元为点对且 (cdr 单元)
  为 t 时表示为格式串，nil 时为的内容串"
  "(text:parse-mtext \"\\\\A1;m{\\\\H0.7x;\\\\S3^;} {\\\\H0.7x;\\\\S^2;\\\\H1.4286x; aN\\\\KN BBB\\\\k  \\\\OOO \\\\oabc  \\\\P\\\\P\\\\fSimSun|b0|i0|c134|p2;sdfaf}\")"
  (if (= (quote str)
      (type str-m))
    (progn (setq flag nil)
      (setq res nil)
      (setq str-list (vl-string->list str-m))
      (while (setq asc% (car str-list))
        (cond (flag (setq fmt% (cons asc% fmt%))
            (cond ((= ";"
                  (chr asc%))
                (setq flag nil)
                (setq flag-char nil)
                (setq lst-res (cons (cons fmt% t)
                    lst-res))
                (setq fmt% nil))
              ((and first (member (chr asc%)
                    (list "{"
                      "}"
                      (chr 92))))
                (setq flag nil)
                (setq fmt% nil)
                (setq str% (cons asc% str%)))
              ((and first (member (chr asc%)
                    (quote ("P"
                        "K"
                        "k"
                        "O"
                        "o"
                        "S"))))
                (setq flag nil)
                (setq flag-char (chr asc%))
                (setq lst-res (cons (cons fmt% t)
                    lst-res))
                (setq fmt% nil))
              (t))
            (setq first nil))
          ((= flag-char "S")
            (cond ((member (chr asc%)
                  (quote ("^")))
                (if str% (setq lst-res (cons (cons str% nil)
                      lst-res)))
                (setq str% nil)
                (setq lst-res (cons (cons (list asc%)
                      t)
                    lst-res)))
              ((member (chr asc%)
                  (quote (";")))
                (setq flag-char nil)
                (if str% (setq lst-res (cons (cons str% nil)
                      lst-res)))
                (setq str% nil)
                (setq lst-res (cons (cons (list asc%)
                      t)
                    lst-res)))
              (t (setq str% (cons asc% str%)))))
          ((member (chr asc%)
              (quote ("{")))
            (setq fmt% (cons asc% fmt%)))
          ((member (chr asc%)
              (quote ("}")))
            (if str% (setq lst-res (cons (cons str% nil)
                  lst-res)))
            (setq str% nil)
            (setq fmt% (cons asc% fmt%))
            (setq lst-res (cons (cons fmt% t)
                lst-res))
            (setq fmt% nil))
          ((= 92 asc%)
            (setq flag t)
            (setq first t)
            (setq fmt% (cons asc% fmt%))
            (if str% (setq lst-res (cons (cons str% nil)
                  lst-res)))
            (setq str% nil))
          (t (setq str% (cons asc% str%))))
        (setq str-list (cdr str-list)))
      (if str% (setq lst-res (cons str% lst-res)))
      (if fmt% (setq lst-res (cons fmt% lst-res)))
      (reverse (mapcar (quote (lambda (x)
              (if (atom (car x))
                (setq x (list x)))
              (cons (vl-list->string (reverse (if (listp (car x))
                      (car x)
                      (list x))))
                (cdr x))))
          lst-res)))
    nil))
```
</details>

---

### text:remove-fmt

**说明**: 去除多行文本中的格式化字符串.

**用法**:
```lisp
(text:remove-fmt str-mtext)
```

**参数**: 1 str-mtext  : 字符串;

**返回值**: 结果字符串

**示例**:
```lisp
(text:remove-fmt "\\A1;m{\\H0.7x;\\S3^;} {\\H0.7x;\\S^2;\\H1.4286x; aN\\KN BBB\\k  \\OOO \\oabc  \\P\\P\\fSimSun|b0|i0|c134|p2;sdfaf}")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun text:remove-fmt (str-mtext / res)
  "去除多行文本中的格式化字符串."
  "结果字符串"
  "(text:remove-fmt \"\\\\A1;m{\\\\H0.7x;\\\\S3^;} {\\\\H0.7x;\\\\S^2;\\\\H1.4286x; aN\\\\KN BBB\\\\k  \\\\OOO \\\\oabc  \\\\P\\\\P\\\\fSimSun|b0|i0|c134|p2;sdfaf}\")"
  (if (setq res (mapcar (quote car)
        (vl-remove-if (quote (lambda (x)
              (cdr x)))
          (text:parse-mtext str-mtext))))
    (apply (quote strcat)
      res)))
```
</details>

---

### text:stringexplode

**用法**:
```lisp
(text:stringexplode str_given)
```

**参数**: 1 str_given  : 字符串;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun text:stringexplode (str_given / strlst_defined strlst_defined2 str_defined i_length str_lst_return e)
    (while (> (strlen str_given)
            0)
        (cond ((and (> (strlen str_given)
                        2)
                    (setq startdefinednum (vl-string-search "%"
                            str_given 0))
                    (= startdefinednum 0))
                (if (setq againdefinednum (vl-string-search "%"
                            str_given (1+ startdefinednum)))
                    (if (member (substr str_given (+ startdefinednum 1)
                                3)
                            (quote ("%%o"
                                    "%%o"
                                    "%%u"
                                    "%%u"
                                    "%%p"
                                    "%%p"
                                    "%%c"
                                    "%%c"
                                    "%%d"
                                    "%%d"
                                    "%%%")))
                        (setq definedstr (substr str_given (+ startdefinednum 1)
                                3)
                            str_lst_return (cons definedstr str_lst_return)
                            str_given (substr str_given 4))
                        (setq definedstr (substr str_given (+ startdefinednum 1)
                                5)
                            str_lst_return (cons definedstr str_lst_return)
                            str_given (substr str_given 6)))))
            ((and (> (strlen str_given)
                        6)
                    (setq startdefinednum (vl-string-search "\\"
                            str_given flagnum))
                    (= startdefinednum 0))
                (if (setq againdefinednum (vl-string-search "u"
                            str_given (1+ startdefinednum)))
                    (if (member (substr str_given (+ startdefinednum 1)
                                8)
                            (quote ("\u+23f61"
                                    "\u+23f62"
                                    "\u+23f63"
                                    "\u+23f64"
                                    "\u+23f65"
                                    "\u+23f66"
                                    "\u+23f67"
                                    "\u+23f68"
                                    "\u+23f69")))
                        (setq definedstr (substr str_given (+ startdefinednum 1)
                                8)
                            str_lst_return (cons definedstr str_lst_return)
                            str_given (substr str_given 9))
                        (setq definedstr (substr str_given (+ startdefinednum 1)
                                7)
                            str_lst_return (cons definedstr str_lst_return)
                            str_given (substr str_given 8)))))
            ((> (ascii (substr str_given 1 1))
                    128)
                (setq str_lst_return (cons (substr str_given 1 2)
                        str_lst_return))
                (setq str_given (substr str_given 3)))
            (t (setq str_lst_return (cons (substr str_given 1 1)
                        str_lst_return))
                (setq str_given (substr str_given 2)))))
    (reverse str_lst_return))
```
</details>

---

## timer

**模块说明**: 专用功能模块

### timer:begin

**说明**: 计时器开始

**用法**:
```lisp
(timer:begin )
```

**参数**: 没有一个

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun timer:begin nil "计时器开始"
  (if (> (@::acadver)
      21.9)
    (setq *timer* (getvar "millisecs"))
    (setq *timer* (getvar "TDUSRTIMER"))))
```
</details>

---

### timer:end

**说明**: 计时器结束。time 开始时间 p 是否打印。

**用法**:
```lisp
(timer:end time p)
```

**参数**: 1 time  : 未识别定义;2 p  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun timer:end (time p / usetime)
  "计时器结束。time 开始时间 p 是否打印。"
  (if (not time)
    (setq time *timer*))
  (if (> (@::acadver)
      21.9)
    (setq usetime (- (getvar "millisecs")
        time))
    (setq usetime (* 86400000 (- (getvar "TDUSRTIMER")
          time))))
  (if p (print (strcat "use time: "
        (rtos usetime 2 0)
        "
        micro second.")))
  usetime)
```
</details>

---

## ui

**模块说明**: 用户界面、对话框

### ui:button-select

**说明**: 显示按钮列表选择面板。选择所需项并返回，无需点击确定。

**用法**:
```lisp
(ui:button-select str-subject lst)
```

**参数**: 1 str-subject  : 字符串;2 lst  : 列表;

**返回值**: 选中的内容

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:button-select (str-subject lst / dcl_fp dcl-tmp dcl_id para% result initget%)
  "显示按钮列表选择面板。\n选择所需项并返回，无需点击确定。"
  "选中的内容"
  ""
  (setq result nil)
  (if lst (if (= 1 (getvar "filedia"))
      (progn (progn (setq dcl-tmp (strcat @:*tmp-path* "tmp-bselect.dcl"))
          (setq dcl_fp (open dcl-tmp "w"))
          (write-line (strcat "select : dialog {"
              "label = \""
              str-subject "\"; "
              ":column {label=\""
              str-subject "\"; ")
            dcl_fp)
          (setq i% 1)
          (setq bt-width 18)
          (foreach opt% lst (write-line (strcat ":button{ fixed_width=true; width="
                (itoa bt-width)
                ";key=\"S_"
                (itoa i%)
                "\";label=\""
                opt% "\";action=\"(setq result \\\""
                  opt% "\\\")(done_dialog)\";}")
              dcl_fp)
            (setq i% (1+ i%)))
          (write-line "} :spacer {} ok_cancel; }"
            dcl_fp)
          (close dcl_fp))
        (setq dcl_id (load_dialog dcl-tmp))
        (if (not (new_dialog "select"
              dcl_id))
          (exit))
        (action_tile "accept"
          "(done_dialog 1)")
        (start_dialog)
        (unload_dialog dcl_id)
        (vl-file-delete dcl-tmp)
        result)
      (progn (setq i% 0)
        (setq opt% ""
          initget% "0")
        (foreach lst% lst (setq opt% (strcat opt% (itoa (1+ i%))
              ":"
              lst% "\n"))
          (setq initget% (strcat initget% "\n              "
              (itoa (1+ i%))))
          (setq i% (1+ i%)))
        (initget 1 initget%)
        (nth (1- (atoi (getkword (strcat "请输入要操作的序号 : \n"
                  opt%))))
          lst)))
    (progn (alert (_ "parameter is nil."))
      nil)))
```
</details>

---

### ui:confirm

**说明**: 确认对话框. 参数：lst-str 单个字符串，或字符串列表。

**用法**:
```lisp
(ui:confirm lst-str)
```

**参数**: 1 lst-str  : 列表;

**返回值**: T or nil

**示例**:
```lisp
(ui:confirm "你遇到真爱了吗?")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:confirm (lst-str / *error* result dcl-fp dcl-tmp)
  "确认对话框. 参数：lst-str 单个字符串，或字符串列表。"
  "T or nil"
  "(ui:confirm \"你遇到真爱了吗?\")"
  (defun *error* (msg)
    (if (= (quote file)
        (type dcl-fp))
      (close dcl-fp))
    (vl-file-delete dcl-tmp)
    (prin1 msg))
  (setq result nil)
  (setq dcl-tmp (strcat @:*prefix* "tmp-confirm.dcl"))
  (setq dcl-fp (open dcl-tmp "w"))
  (write-line "confirm :dialog{label=\"Confirm\";:spacer{}:column{"
    dcl-fp)
  (if (not (listp lst-str))
    (setq lst-str (list lst-str)))
  (foreach str (mapcar (quote @:to-string)
      lst-str)
    (write-line (strcat ":text{label="
        (chr 34)
        (string::subst-dqm str)
        "\";}")
      dcl-fp))
  (write-line "} :spacer{} ok_cancel;}"
    dcl-fp)
  (close dcl-fp)
  (setq dcl_id (load_dialog dcl-tmp))
  (if (not (new_dialog "confirm"
        dcl_id ""))
    (exit))
  (action_tile "accept"
    "(setq result T)(done_dialog 1)")
  (start_dialog)
  (unload_dialog dcl_id)
  (vl-file-delete dcl-tmp)
  result)
```
</details>

---

### ui:confirm1

**说明**: 确认对话框1. 参数：lst-str 单个字符串，或字符串列表。参数 str-Yes-No 字符串用 - 分隔成两部分，前面为accept,后面为 Cancel.如 是-否，愿意-不愿意。

**用法**:
```lisp
(ui:confirm1 lst-str str-yes-no)
```

**参数**: 1 lst-str  : 列表;2 str-yes-no  : 字符串;

**返回值**: T or nil

**示例**:
```lisp
(ui:confirm1 '("你家门口有两双鞋。"      "一双是你的。"      "另一双也是你的。"      "你感觉孤独吗？")    "是-否")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:confirm1 (lst-str str-yes-no / *error* result dcl-fp dcl-tmp)
  "确认对话框1.\n 参数：lst-str 单个字符串，或字符串列表。\n参数 str-Yes-No 字符串用 - 分隔成两部分，前面为accept,后面为 Cancel.如 是-否，愿意-不愿意。"
  "T or nil"
  "(ui:confirm1 '(\"你家门口有两双鞋。\"\n      \"一双是你的。\"\n      \"另一双也是你的。\"\n      \"你感觉孤独吗？\")\n    \"是-否\")"
  (setq @:this (quote ui:confirm1))
  (defun *error* (msg)
    (if (= (quote file)
        (type dcl-fp))
      (close dcl-fp))
    (vl-file-delete dcl-tmp)
    (@:*error* msg))
  (if (= (quote str)
      (type lst-str))
    (setq lst-str (list lst-str)))
  (dcl:dialog "confirm")
  (dcl:mtext "mt"
    (length lst-str)
    (max (apply (quote max)
        (mapcar (quote string:bytelength)
          lst-str))
      50))
  (dcl:end-dialog str-yes-no)
  "3. Control 创建控制流程"
  (dcl:new "confirm")
  (set_tile "title"
    "请确认以下事项")
  (dcl:set-mtext "mt"
    (string:from-list lst-str "\n"))
  "6. Show dialog 显示并进行交互"
  (if (> (dcl:show)
      0)
    t nil))
```
</details>

---

### ui:dyndraw

**说明**: 动态绘图，ents随光标移动，按左键定位，右键删除

**用法**:
```lisp
(ui:dyndraw ents pt-base)
```

**参数**: 1 ents  : 图元列表;2 pt-base  : 单个2D/3D坐标点;

**示例**:
```lisp
(ui:dyndraw (ssget)(getpoint))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:dyndraw (ents pt-base / flag r *error*)
  "动态绘图，ents随光标移动，按左键定位，右键删除"
  ""
  "(ui:dyndraw (ssget)(getpoint))"
  (defun *error* (msg)
    (if ents (progn (if (= (quote ename)
            (type ents))
          (setq ents (list ents)))
        (if (= (quote pickset)
            (type ents))
          (setq ents (pickset:to-list ents)))
        (if (p:ename-listp ents)
          (mapcar (quote entdel)
            ents))))
    (princ msg))
  (if (= (quote ename)
      (type ents))
    (setq ents (list ents)))
  (if (= (quote pickset)
      (type ents))
    (setq ents (pickset:to-list ents)))
  (setq flag t)
  (if (p:ename-listp ents)
    (progn (while flag (setq gr (grread t 16))
        (cond ((= 3 (car gr))
            "按下鼠标左键"
            (setq flag nil))
          ((or (= 25 (car gr))
              (= 11 (car gr)))
            "按下鼠标右键"
            (mapcar (quote entdel)
              ents)
            (setq ents nil)
            (setq flag nil))
          ((= 5 (car gr))
            "移动鼠标"
            (mapcar (function (lambda (x)
                  (vla-move (e2o x)
                    (point:to-ax pt-base)
                    (point:to-ax (cadr gr)))))
              ents)
            (setq pt-base (cadr gr)))
          (t "其它情况"
            (princ gr))))
      ents)))
```
</details>

---

### ui:dynquery

**说明**: 动态查询。显示 func 返回的文本列表。func 是对光标所在图元进行的运算结果。n形式如 '(lambda (x) (list (entity:getdxf x 0)))

**用法**:
```lisp
(ui:dynquery func)
```

**参数**: 1 func  : 未识别定义;

**返回值**: nil,执行过程动态显示用户定义的内容

**示例**:
```lisp
(ui:dynquery '(lambda (x) (list (entity:getdxf x '(0 8)))))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:dynquery (func / *error* dxf fx add_background add_box add_text display olderr oldos oldfill ss pd gr pt ent entold)
  "动态查询。显示 func 返回的文本列表。func 是对光标所在图元进行的运算结果。\n形式如 '(lambda (x)
    (list (entity:getdxf x 0)))"
  "nil,执行过程动态显示用户定义的内容"
  "(ui:dynquery '(lambda (x)
      (list (entity:getdxf x '(0 8)))))"
  (defun *error* (msg / i%)
    (if (> (sslength ss)
        0)
      (mapcar (quote entdel)
        (pickset:to-list ss)))
    (print msg)
    (pop-var)
    (princ))
  (defun fx (ang)
    (cond ((>= (/ pi 2)
          ang 0)
        (list pi (+ pi (/ pi 2))
          1))
      ((>= pi ang (/ pi 2))
        (list 0 (+ pi (/ pi 2))
          1))
      ((>= (+ pi (/ pi 2))
          ang pi)
        (list 0 (/ pi 2)
          0))
      ((>= (* 2 pi)
          ang (+ pi (/ pi 2)))
        (list pi (/ pi 2)
          0))))
  (defun add_background (p1 p2 p3 p4)
    (entmakex (list (cons 0 "SOLID")
        (cons 100 "AcDbEntity")
        (cons 62 8)
        (cons 100 "AcDbTrace")
        (cons 10 p1)
        (cons 11 p4)
        (cons 12 p2)
        (cons 13 p3))))
  (defun add_box (pts / dxfcodes)
    (setq dxfcodes (list (cons 0 "LWPOLYLINE")
        (cons 100 "AcDbEntity")
        (cons 100 "AcDbPolyline")
        (cons 62 2)
        (cons 90 (length pts))
        (cons 70 1)
        (cons 43 0)
        (cons 38 0.0)
        (cons 39 0.0)))
    (foreach pt% pts (setq dxfcodes (append dxfcodes (list (cons 10 pt%)
            (cons 40 0.0)
            (cons 41 0.0)
            (cons 42 0.0)
            (cons 91 0)))))
    (entmakex (append dxfcodes (list (quote (210 0.0 0.0 1.0))))))
  (defun add_text (pt h ang txt style jus)
    (entmakex (list (cons 0 "TEXT")
        (cons 100 "AcDbEntity")
        (cons 62 2)
        (cons 100 "AcDbText")
        (if (= jus 0)
          (cons 10 pt)
          (list 10 0.0 0.0 0.0))
        (cons 40 h)
        (cons 1 txt)
        (cons 50 ang)
        (cons 7 style)
        (cons 72 (cond ((= jus 0)
              0)
            ((= jus 1)
              1)
            ((= jus 2)
              1)
            ((= jus 3)
              2)))
        (if (= jus 0)
          (list 11 0.0 0.0 0.0)
          (cons 11 pt))
        (cons 100 "AcDbText")
        (cons 73 (cond ((= jus 0)
              0)
            ((= jus 1)
              2)
            ((= jus 2)
              3)
            ((= jus 3)
              2))))))
  (defun display (ent func / obj laynm name st1 st2 st3 lst h ang n box-pts text-style)
    (setq text-style "vitalhz")
    (if (null (tblsearch "style"
          text-style))
      (setq text-style (getvar "textstyle")))
    (setq obj (vlax-ename->vla-object ent))
    (setq laynm (strcat "图层:"
        (entity:getdxf ent 8)))
    (setq name (entity:getdxf ent 0))
    (setq lst (func ent))
    (if (= (quote str)
        (type lst))
      (setq lst (list lst)))
    (if (or (null lst)
        (not (listp lst)))
      (setq lst (list "Error! Not defined function.")))
    (setq lst (vl-remove nil lst))
    (setq lst (mapcar (quote @:to-string)
        lst))
    (setq ss (ssadd))
    (setq h (/ (getvar "viewsize")
        40))
    (setq ang (fx (angle (getvar "viewctr")
          pt)))
    (setq n (* 1.4 (1+ (/ (apply (quote max)
              (mapcar (quote strlen)
                lst))
            2.0))))
    (setq box-pts (list pt (setq st1 (polar pt (* 1.5 pi)
            (+ h (* 1.8 h (1+ (length lst))))))
        (polar st1 0 (* n h))
        (polar pt 0 (* n h))))
    (ssadd (apply (quote add_background)
        box-pts)
      ss)
    (ssadd (add_box box-pts)
      ss)
    (setq st2 (polar pt 0 (/ (* n h)
          2)))
    (setq n -1)
    (repeat (length lst)
      (ssadd (add_text (setq st2 (polar st2 (* 1.5 pi)
              (* 1.8 h)))
          h 0 (nth (setq n (1+ n))
            lst)
          text-style 1)
        ss)))
  (push-var)
  (command "_.undo"
    "_m")
  (prompt "\n*** Move cursor to entity for show infomation！***")
  (setvar "osmode"
    0)
  (setvar "fillmode"
    1)
  (setvar "cmdecho"
    0)
  (setq ss (ssadd))
  (while (and (setq gr (grread t 1))
      (= (car gr)
        5))
    (if (and (listp (cadr gr))
        (> (length (cadr gr))
          1)
        (setq pt (cadr gr))
        (setq respt (nentselp (cadr gr)))
        (setq ent (if (= (type (last (last respt)))
              (quote ename))
            (last (last respt))
            (car respt)))
        (not (equal ent entold))
        (not (ssmemb ent ss)))
      (progn (if entold (redraw entold 4))
        (if (> (sslength ss)
            0)
          (mapcar (quote entdel)
            (pickset:to-list ss)))
        (redraw ent 3)
        (display ent (eval func))
        (setq entold ent))))
  (if entold (redraw entold 4))
  (if (> (sslength ss)
      0)
    (mapcar (quote entdel)
      (pickset:to-list ss)))
  (pop-var)
  (princ))
```
</details>

---

### ui:dynrotate

**说明**: 动态绘图，ents随光标旋转，按左键定位，右键删除

**用法**:
```lisp
(ui:dynrotate ents pt-base)
```

**参数**: 1 ents  : 图元列表;2 pt-base  : 单个2D/3D坐标点;

**示例**:
```lisp
(ui:dynrotate (ssget)(getpoint))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:dynrotate (ents pt-base / flag r)
  "动态绘图，ents随光标旋转，按左键定位，右键删除"
  ""
  "(ui:dynrotate (ssget)(getpoint))"
  (if (= (quote ename)
      (type ents))
    (setq ents (list ents)))
  (if (= (quote pickset)
      (type ents))
    (setq ents (pickset:to-list ents)))
  (setq flag t)
  (setq ang-base 0)
  (while flag (setq gr (grread t 16))
    (cond ((= 3 (car gr))
        "按下鼠标左键"
        (setq flag nil))
      ((or (= 25 (car gr))
          (= 11 (car gr)))
        "按下鼠标右键"
        (mapcar (quote entdel)
          ents)
        (setq flag nil))
      ((= 5 (car gr))
        "移动鼠标"
        (mapcar (function (lambda (x)
              (vla-rotate (e2o x)
                (point:to-ax pt-base)
                (- (angle pt-base (cadr gr))
                  ang-base))))
          ents)
        (setq ang-base (angle pt-base (cadr gr)))
        (princ "\n")
        (princ (angtos ang-base 0 3)))
      (t "其它情况"
        (princ gr))))
  (princ))
```
</details>

---

### ui:dynscale

**说明**: 动态绘图，ents随光标缩放，按左键定位，右键删除

**用法**:
```lisp
(ui:dynscale ents pt-base)
```

**参数**: 1 ents  : 图元列表;2 pt-base  : 单个2D/3D坐标点;

**示例**:
```lisp
(ui:dynscale (ssget)(getpoint))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:dynscale (ents pt-base / flag r)
  "动态绘图，ents随光标缩放，按左键定位，右键删除"
  ""
  "(ui:dynscale (ssget)(getpoint))"
  (if (= (quote ename)
      (type ents))
    (setq ents (list ents)))
  (if (= (quote pickset)
      (type ents))
    (setq ents (pickset:to-list ents)))
  (setq scale-base 1)
  (setq flag t)
  (while flag (setq gr (grread t 16))
    (cond ((= 3 (car gr))
        "按下鼠标左键"
        (setq flag nil))
      ((or (= 25 (car gr))
          (= 11 (car gr)))
        "按下鼠标右键"
        (mapcar (quote entdel)
          ents)
        (setq flag nil))
      ((= 5 (car gr))
        "移动鼠标"
        (mapcar (function (lambda (x)
              (vla-scaleentity (e2o x)
                (point:to-ax pt-base)
                (/ (* 0.001 (distance pt-base (cadr gr)))
                  scale-base))))
          ents)
        (setq scale-base (* 0.001 (distance pt-base (cadr gr)))))
      (t "其它情况"
        (princ gr))))
  (princ))
```
</details>

---

### ui:getdist

**说明**: 当按下键盘数字键时，返回数值，当鼠标左键点取文字或标注时，取文字或标注的值。

**用法**:
```lisp
(ui:getdist msg)
```

**参数**: 1 msg  : 提示信息;

**返回值**: Number

**示例**:
```lisp
(ui:getdist "Please input number or select text/dimension")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:getdist (msg / flag sn ents ss)
  "当按下键盘数字键时，返回数值，当鼠标左键点取文字或标注时，取文字或标注的值。"
  "Number"
  "(ui:getdist \"Please input number or select text/dimension\")"
  (princ msg)
  (setq sn "")
  (setq flag t)
  (while flag (setq gr (grread t 16))
    "处理输入"
    (cond ((= 2 (car gr))
        "按下了键盘按键"
        (cond ((member (cadr gr)
              (vl-string->list "0123456789."))
            "持续输入数字键"
            (setq sn (strcat sn (chr (cadr gr))))
            (princ (chr (cadr gr))))
          ((member (cadr gr)
              (quote (13 32)))
            "回车 或空格，返回输入的值"
            (princ "\n")
            (setq flag nil))))
      ((= 3 (car gr))
        "按下鼠标左键，选中图元，读值"
        (setq ents (pickset:to-list (ssget (cadr gr)
              (quote ((0 . "DIM*,TEXT"))))))
        (if ents (progn (if (/= (entity:getdxf (car ents)
                  1)
                "")
              (setq sn (entity:getdxf (car ents)
                  1))
              (if (entity:getdxf (car ents)
                  42)
                (setq sn (entity:getdxf (car ents)
                    42))))
            (setq flag nil))))
      ((= 5 (car gr))
        "移动鼠标,高亮图元"
        (if ss (redraw (ssname ss 0)
            4))
        (setq ss (ssget (cadr gr)
            (quote ((0 . "DIM*,TEXT")))))
        (if ss (redraw (ssname ss 0)
            3)))))
  (if (= (quote str)
      (type sn))
    (atof sn)
    (if (numberp sn)
      sn)))
```
</details>

---

### ui:getstring

**说明**: 当按下键盘字符键时，返回字符串，当鼠标左键点取文字或标注时，取文字或标注的值。

**用法**:
```lisp
(ui:getstring msg)
```

**参数**: 1 msg  : 提示信息;

**返回值**: String

**示例**:
```lisp
(ui:getstring "Please input string or select text/dimension")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:getstring (msg / flag sn ents ss)
  "当按下键盘字符键时，返回字符串，当鼠标左键点取文字或标注时，取文字或标注的值。"
  "String"
  "(ui:getstring \"Please input string or select text/dimension\")"
  (princ msg)
  (setq sn "")
  (setq flag t)
  (while flag (setq gr (grread t 16))
    "处理输入"
    (cond ((= 2 (car gr))
        "按下了键盘按键"
        (cond ((member (cadr gr)
              (quote (13 32)))
            "回车 或空格，返回输入的值"
            (princ "\n")
            (setq flag nil))
          (t "持续输入字符"
            (setq sn (strcat (chr (cadr gr))
                (getstring (chr (cadr gr)))))
            (setq flag nil))))
      ((= 3 (car gr))
        "按下鼠标左键，选中图元，读值"
        (setq ents (pickset:to-list (ssget (cadr gr)
              (quote ((0 . "DIM*,TEXT"))))))
        (if ents (progn (if (/= (entity:getdxf (car ents)
                  1)
                "")
              (setq sn (entity:getdxf (car ents)
                  1))
              (if (entity:getdxf (car ents)
                  42)
                (setq sn (entity:getdxf (car ents)
                    42))))
            (if (> (strlen sn)
                0)
              (setq flag nil)))))
      ((= 5 (car gr))
        "移动鼠标,高亮图元"
        (if ss (redraw (ssname ss 0)
            4))
        (setq ss (ssget (cadr gr)
            (quote ((0 . "DIM*,TEXT")))))
        (if ss (redraw (ssname ss 0)
            3)))))
  sn)
```
</details>

---

### ui:input

**说明**: 显示输入一个或多个文本输入的面板，返回所有文本框的值。lst 由一个或多个列表组成     每个元素由 (label 默认值 说明 是否密文/列表及列表默认序号(以0开始))  组成 。label 不可省略，默认值、说明和密文/序号 可省略。当为 <*> 时，为特殊处理符号，如  水平线， 单行文本。默认值(第二项):    当没有默认值及以后各项时，显示为文本编辑框    当为数值型和字符串时，且第4项不为列表时显示文本编辑框    当为数值型和字符串时，且第4项为列表时显示下拉菜单    当为数值型和字符串时，且第4项为 T 时显示密码编辑框    当为 T or nil 时，显示复选框，    当为列表时，显示下拉菜单。第4项为数字时，为列表默认值索引号。

**用法**:
```lisp
(ui:input str-subject lst)
```

**参数**: 1 str-subject  : 字符串;2 lst  : 列表;

**返回值**: 所有输入框的 label 和 值 组成的点对表，或 nil

**示例**:
```lisp
(ui:input "请输入以下内容："    '(("Name1")("Name2"        "VitalGG"        "带默认值的输入框")      ("Pass1"        "123456"        "密码框"        T)      ("Bool:"        T "真假值")      ("Popup1:"        ("one"          "two"          "three")        "下拉列表")("Popup2:"        3 "下拉列表2"        (1 2 3 4 5))))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:input (str-subject lst / *error* dcl_fp dcl-tmp dcl_id para% set-result result initget%)
  "显示输入一个或多个文本输入的面板，返回所有文本框的值。\nlst 由一个或多个列表组成 \n    每个元素由 (label 默认值 说明 是否密文/列表及列表默认序号(以0开始))\n  组成 。\nlabel 不可省略，默认值、说明和密文/序号 可省略。当为 <*> 时，为特殊处理符号，如 <hr> 水平线，<text> 单行文本。\n默认值(第二项):\n    当没有默认值及以后各项时，显示为文本编辑框\n    当为数值型和字符串时，且第4项不为列表时显示文本编辑框\n    当为数值型和字符串时，且第4项为列表时显示下拉菜单\n    当为数值型和字符串时，且第4项为 T 时显示密码编辑框\n    当为 T or nil 时，显示复选框，\n    当为列表时，显示下拉菜单。第4项为数字时，为列表默认值索引号。\n"
  "所有输入框的 label 和 值 组成的点对表，或 nil"
  "(ui:input \"请输入以下内容：\"\n    '((\"Name1\")(\"Name2\"\n        \"VitalGG\"\n        \"带默认值的输入框\")\n      (\"Pass1\"\n        \"123456\"\n        \"密码框\"\n        T)\n      (\"Bool:\"\n        T \"真假值\")\n      (\"Popup1:\"\n        (\"one\"\n          \"two\"\n          \"three\")\n        \"下拉列表\")(\"Popup2:\"\n        3 \"下拉列表2\"\n        (1 2 3 4 5))))"
  (defun *error* (msg)
    (if (= (quote file)
        (type dcl_fp))
      (close dcl_fp))
    (princ msg)
    (princ))
  (setq width-label (+ 4 (apply (quote max)
        (mapcar (quote (lambda (x)
              (strlen (car x))))
          lst))))
  (setq width-value (+ 2 (apply (quote max)
        (mapcar (quote (lambda (x)
              (strlen (@:to-string (cadr x)))))
          (vl-remove-if (quote (lambda (x)
                (or (null (cadr x))
                  (wcmatch (car x)
                    "<*>"))))
            lst)))))
  (if (> 5 width-value)
    (setq width-value 20))
  (setq width-desc (apply (quote max)
      (mapcar (quote (lambda (x)
            (strlen (@:to-string (caddr x)))))
        (vl-remove-if (quote (lambda (x)
              (null (caddr x))))
          lst))))
  (cond (is-gcad (setq width-desc (fix (* 1.5 width-desc))))
    ((< (@:acadver)
        22)
      (setq width-desc (fix (* 1.5 width-desc)))
      (setq width-desc (if (> width-desc 24)
          (fix (* width-desc 0.8))
          width-desc)))
    ((< (@:acadver)
        24)
      (setq width-desc (fix (* 1.0 width-desc)))
      (setq width-desc (fix (* width-desc 0.5))))
    (t (setq width-desc (fix (* 2.0 width-desc)))
      (setq width-desc (if (> width-desc 27)
          (fix (* width-desc 1.0))
          width-desc))))
  (defun set-result (/ i%)
    (setq result (quote nil))
    (setq i% 1)
    (foreach opt% lst (cond ((or (= 1 (length opt%))
            (and (< 1 (length opt%))
              (member (type-of (cadr opt%))
                (quote (int real str)))))
          (setq result (cons (@:to-type (get_tile (strcat "S_"
                    (itoa i%)))
                (if (cadr opt%)
                  (type-of (cadr opt%))
                  (quote str)))
              result)))
        ((or (= (cadr opt%)
              t)
            (= (cadr opt%)
              nil))
          (setq result (cons (if (= "1"
                  (get_tile (strcat "S_"
                      (itoa i%))))
                t nil)
              result)))
        ((listp (cadr opt%))
          (setq result (cons (nth (atoi (get_tile (strcat "S_"
                      (itoa i%))))
                (cadr opt%))
              result))))
      (setq i% (1+ i%))))
  (if lst (if (= 1 (getvar "filedia"))
      (progn (setq dcl-tmp (strcat @:*tmp-path* "tmp-input.dcl"))
        (setq dcl_fp (open dcl-tmp "w"))
        (write-line (strcat "input : dialog {"
            "label = \""
            str-subject "\"; "
            ":column {label=\""
            str-subject "\"; alignment=left;")
          dcl_fp)
        (setq i% 1)
        (setq bt-width 28)
        (foreach opt% lst (cond ((and (> (length opt%)
                  3)
                (= (quote list)
                  (type (nth 3 opt%))))
              (write-line (strcat ":row{:popup_list { fixed_width=true; width="
                  (itoa bt-width)
                  ";key=\"S_"
                  (itoa i%)
                  "\";"
                  "label=\""
                  (car opt%)
                  "\";"
                  (if (vl-position (cadr opt%)
                      (nth 3 opt%))
                    (strcat "value=\""
                      (itoa (vl-position (cadr opt%)
                          (nth 3 opt%)))
                      "\";")
                    "")
                  "}"
                  (if (or (caddr opt%)
                      (> width-desc 0))
                    (strcat ":text{value=\""
                      (@:to-string (caddr opt%))
                      "\";fixed_width=true; width="
                      (itoa width-desc)
                      ";alignment=left;}")
                    "")
                  "}")
                dcl_fp))
            ((wcmatch (car opt%)
                "<*>")
              (cond ((= "<hr>"
                    (car opt%))
                  (write-line (strcat ":image{ height=0.1; color=250; fixed_height=true;}")
                    dcl_fp))
                ((= "<text>"
                    (car opt%))
                  (write-line (strcat ":text{label=\""
                      (@:to-string (cadr opt%))
                      "\";}")
                    dcl_fp))
                ((= "<button>"
                    (car opt%))
                  (write-line (strcat ":button{label=\""
                      (@:to-string (cadr opt%))
                      "\";}")
                    dcl_fp))))
            ((or (= 1 (length opt%))
                (and (> (length opt%)
                    1)
                  (member (type-of (cadr opt%))
                    (quote (int real str)))))
              (write-line (strcat ":row{:text{ value=\""
                  (car opt%)
                  "\";width="
                  (itoa width-label)
                  ";fixed_width=true;}"
                  ":edit_box { fixed_width=true; width="
                  (itoa width-value)
                  ";key=\"S_"
                  (itoa i%)
                  "\";"
                  "value=\""
                  (if (cadr opt%)
                    (@:string-subst "\\\\"
                      "\\"
                      (@:to-string (cadr opt%)))
                    "")
                  "\";"
                  (if (nth 3 opt%)
                    "password_char=\"*\";"
                    "")
                  "}"
                  (if (or (caddr opt%)
                      (> width-desc 0))
                    (strcat ":text{value=\""
                      (@:to-string (caddr opt%))
                      "\";fixed_width=true; width="
                      (itoa width-desc)
                      ";alignment=left;}")
                    "")
                  "}")
                dcl_fp))
            ((or (= (cadr opt%)
                  t)
                (= (cadr opt%)
                  nil))
              (write-line (strcat ":row{:text{value=\""
                  (car opt%)
                  "\"; fixed_width=true;width="
                  (itoa width-label)
                  ";} :toggle{fixed_width=true;width="
                  (itoa width-value)
                  ";key=\"S_"
                  (itoa i%)
                  "\";"
                  "label=\"\";value=\""
                  (if (cadr opt%)
                    "1"
                    "0")
                  "\";alignment=left;"
                  "}"
                  (if (or (caddr opt%)
                      (> width-desc 0))
                    (strcat ":text{value=\""
                      (@:to-string (caddr opt%))
                      "\";fixed_width=true; width="
                      (itoa width-desc)
                      ";alignment=left;}")
                    "")
                  "}")
                dcl_fp))
            ((= (quote list)
                (type-of (cadr opt%)))
              (write-line (strcat ":row{:popup_list { fixed_width=true; width="
                  (itoa bt-width)
                  ";key=\"S_"
                  (itoa i%)
                  "\";"
                  "label=\""
                  (car opt%)
                  "\";"
                  (if (= (quote int)
                      (type-of (nth 3 opt%)))
                    (strcat "value=\""
                      (itoa (nth 3 opt%))
                      "\";")
                    "")
                  "}"
                  (if (or (caddr opt%)
                      (> width-desc 0))
                    (strcat ":text{value=\""
                      (@:to-string (caddr opt%))
                      "\";fixed_width=true; width="
                      (itoa width-desc)
                      ";alignment=left;}")
                    "")
                  "}")
                dcl_fp)))
          (setq i% (1+ i%)))
        (write-line "} :spacer {} ok_cancel; }"
          dcl_fp)
        (close dcl_fp)
        (setq dcl_id (load_dialog dcl-tmp))
        (if (not (new_dialog "input"
              dcl_id))
          (exit))
        (setq i% 1)
        (foreach opt% lst (if (listp (cadr opt%))
            (progn (start_list (strcat "S_"
                  (itoa i%)))
              (mapcar (quote add_list)
                (mapcar (quote @:to-string)
                  (cadr opt%)))
              (end_list)))
          (setq i% (1+ i%)))
        (setq i% 1)
        (foreach opt% lst (if (and (nth 3 opt%)
              (listp (nth 3 opt%)))
            (progn (start_list (strcat "S_"
                  (itoa i%)))
              (mapcar (quote add_list)
                (mapcar (quote @:to-string)
                  (nth 3 opt%)))
              (end_list)))
          (setq i% (1+ i%)))
        (action_tile "accept"
          "(set-result)(done_dialog 1)")
        (start_dialog)
        (unload_dialog dcl_id)
        (vl-file-delete dcl-tmp)
        (vl-remove-if (quote (lambda (x)
              (wcmatch (car x)
                "<*>")))
          (mapcar (quote cons)
            (mapcar (quote car)
              lst)
            (reverse result)))))
    (progn (alert (_ "parameter is nil."))
      nil)))
```
</details>

---

### ui:kword

**说明**: 下拉列表选择,当 lst-str字符串表中有空格和汉字，提示信息将显示选择序号

**用法**:
```lisp
(ui:kword title lst-str)
```

**参数**: 1 title  : 未识别定义;2 lst-str  : 列表;

**返回值**: 选中的字符串

**示例**:
```lisp
(ui:kword "Please select:" '("a""b""c"))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:kword (title lst-str / olddyn olddynprompt *error* short?)
  "下拉列表选择,当 lst-str字符串表中有空格和汉字，提示信息将显示选择序号"
  "选中的字符串"
  "(ui:kword \"Please select:\"
    '(\"a\"\"b\"\"c\"))"
  (defun *error* (msg)
    (if olddyn (setvar "dynmode"
        olddyn))
    (if olddynprompt (setvar "dynprompt"
        olddynprompt))
    (princ msg)
    (princ))
  (if (and (listp lst-str)
      (> (length lst-str)
        0)
      (p:stringp title))
    (progn (setq short? t)
      (mapcar (quote (lambda (x / lst)
            (setq lst (string:s2l-ansi x))
            (if (or (member 32 lst)
                (> (apply (quote max)
                    lst)
                  128))
              (setq short? nil))))
        lst-str)
      (if (setq olddynprompt (getvar "dynprompt"))
        (setvar "dynprompt"
          1))
      (if (setq olddyn (getvar "dynmode"))
        (setvar "dynmode"
          3))
      (setq lst-n (mapcar (quote itoa)
          (list:range 1 (length lst-str)
            1)))
      (initget (string:from-list (if short? lst-str lst-n)
          "
          "))
      (setq res (getkword (strcat title "["
            (string:from-list (if short? lst-str (mapcar (quote (lambda (x y)
                      (strcat x "("
                          y ")")))
                  lst-str lst-n))
              "/")
            "]")))
      (if olddyn (setvar "dynmode"
          olddyn))
      (if olddynprompt (setvar "dynprompt"
          olddynprompt))
      (if short? res (cdr (assoc res (mapcar (quote cons)
              lst-n lst-str)))))))
```
</details>

---

### ui:progress

**说明**: 提供一个进度条功能函数，current 是当前进度   max-number 是进度总量调用完成之后，可以使用(GRTEXT)  清除进度

**用法**:
```lisp
(ui:progress current max-number)
```

**参数**: 1 current  : 未识别定义;2 max-number  : 未识别定义;

**返回值**: 无返回值

**示例**:
```lisp
(SETQ CURRENT 0)(repeat 1000 (UI:PROGRESS (setq CURRENT (1+ CURRENT))      1000))(grtext)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:progress (current max-number)
  "\n提供一个进度条功能函数，current 是当前进度   max-number 是进度总量\n调用完成之后，可以使用(GRTEXT)\n  清除进度\n"
  "无返回值"
  "\n(SETQ CURRENT 0)\n(repeat 1000 (UI:PROGRESS (setq CURRENT (1+ CURRENT))\n      1000))\n(grtext)\n"
  (require (quote music-die:multi-element))
  (grtext -1 (strcat (substr "████████████████████"
        1 (* 2 (fix (* 10 (/ current max-number 1.0)))))
      (nth (fix (* 7 (- (* 10 (/ current max-number 1.0))
              (fix (* 10 (/ current max-number 1.0))))))
        (quote ("▏"
            "▎"
            "▍"
            "▌"
            "▋"
            "▊"
            "▉")))
      (apply (quote strcat)
        (music-die:multi-element "\n          "
          (- 20 (* 2 (fix (* 10 (/ current max-number 1.0)))))))
      (rtos current)
      "/"
      (rtos max-number))))
```
</details>

---

### ui:progress1

**说明**: 基于 ACET 的状态栏进度条.

**用法**:
```lisp
(ui:progress1 ratio)
```

**参数**: 1 ratio  : 未识别定义;

**示例**:
```lisp
(ui:progress1 30)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:progress1 (ratio)
  "基于 ACET 的状态栏进度条."
  ""
  "(ui:progress1 30)"
  (if (type acet-ui-progress)
    (progn (if (null progress)
        (setq progress (acet-ui-progress "已经完成"
            100)))
      (acet-ui-progress ratio)
      (if (>= ratio 100)
        (setq progress (acet-ui-progress))))
    (princ "没有安装 ExpressTools")))
```
</details>

---

### ui:select

**说明**: 显示列表选择面板，选择所需项并返回。

**用法**:
```lisp
(ui:select str-subject lst)
```

**参数**: 1 str-subject  : 字符串;2 lst  : 列表;

**返回值**: 选中的内容

**示例**:
```lisp
(ui:select "请选择你需要操作的项"    '("我愿意"      "不愿意"       "你是一个好人"))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:select (str-subject lst / dcl_fp dcl-tmp dcl_id para% set-result result initget%)
  "显示列表选择面板，选择所需项并返回。"
  "选中的内容"
  "(ui:select \"请选择你需要操作的项\"\n    '(\"我愿意\"\n      \"不愿意\"\n       \"你是一个好人\"))"
  (defun set-result (/ i%)
    (setq i% 1)
    (foreach opt% lst (if (= "1"
          (get_tile (strcat "S_"
              (itoa i%))))
        (setq result (nth (1- i%)
            lst)))
      (setq i% (1+ i%))))
  (if lst (if (= 1 (getvar "filedia"))
      (progn (progn (setq dcl-tmp (strcat @:*tmp-path* "tmp-select.dcl"))
          (setq dcl_fp (open dcl-tmp "w"))
          (write-line (strcat "select : dialog {"
              "label = \""
              str-subject "\"; "
              ":radio_column {label=\""
              str-subject "\"; ")
            dcl_fp)
          (setq i% 1)
          (setq bt-width 18)
          (foreach opt% lst (write-line (strcat ":radio_button{ fixed_width=true; width="
                (itoa bt-width)
                ";key=\"S_"
                (itoa i%)
                "\";label=\""
                opt% "\";}")
              dcl_fp)
            (setq i% (1+ i%)))
          (write-line "} :spacer {} ok_cancel; }"
            dcl_fp)
          (close dcl_fp))
        (setq dcl_id (load_dialog dcl-tmp))
        (if (not (new_dialog "select"
              dcl_id))
          (exit))
        (action_tile "accept"
          "(set-result)(done_dialog 1)")
        (start_dialog)
        (unload_dialog dcl_id)
        (vl-file-delete dcl-tmp)
        result)
      (progn (setq i% 0)
        (setq opt% ""
          initget% "0")
        (foreach lst% lst (setq opt% (strcat opt% (itoa (1+ i%))
              ":"
              lst% "\n"))
          (setq initget% (strcat initget% "\n              "
              (itoa (1+ i%))))
          (setq i% (1+ i%)))
        (initget 1 initget%)
        (nth (1- (atoi (getkword (strcat "请输入要操作的序号 : \n"
                  opt%))))
          lst)))
    (progn (alert (_ "parameter is nil."))
      nil)))
```
</details>

---

### ui:select-multi

**说明**: 显示列表选择面板，选择多个所需项并返回。

**用法**:
```lisp
(ui:select-multi str-subject lst)
```

**参数**: 1 str-subject  : 字符串;2 lst  : 列表;

**返回值**: 选中的内容

**示例**:
```lisp
(ui:select-multi "请选择你喜欢的人"    '("AB"      "Lisa"       "VitalGG"))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:select-multi (str-subject lst / dcl_fp dcl-tmp dcl_id para% set-result result initget%)
  "显示列表选择面板，选择多个所需项并返回。"
  "选中的内容"
  "(ui:select-multi \"请选择你喜欢的人\"\n    '(\"AB\"\n      \"Lisa\"\n       \"VitalGG\"))"
  (defun set-result (/ i%)
    (setq result (quote nil))
    (setq i% 1)
    (foreach opt% lst (if (= "1"
          (get_tile (strcat "S_"
              (itoa i%))))
        (setq result (cons (nth (1- i%)
              lst)
            result)))
      (setq i% (1+ i%))))
  (if lst (if (= 1 (getvar "filedia"))
      (progn (setq dcl-tmp (strcat @:*tmp-path* "tmp-select.dcl"))
        (setq dcl_fp (open dcl-tmp "w"))
        (write-line (strcat "select : dialog {"
            "label = \""
            str-subject "\"; "
            ":column {label=\""
            str-subject "\"; ")
          dcl_fp)
        (setq i% 1)
        (setq bt-width 18)
        (foreach opt% lst (write-line (strcat ":toggle{ fixed_width=true; width="
              (itoa bt-width)
              ";key=\"S_"
              (itoa i%)
              "\";label=\""
              opt% "\";}")
            dcl_fp)
          (setq i% (1+ i%)))
        (write-line "} :spacer {} ok_cancel; }"
          dcl_fp)
        (close dcl_fp)
        (setq dcl_id (load_dialog dcl-tmp))
        (if (not (new_dialog "select"
              dcl_id))
          (exit))
        (action_tile "accept"
          "(set-result)(done_dialog 1)")
        (start_dialog)
        (unload_dialog dcl_id)
        (vl-file-delete dcl-tmp)
        result))
    (progn (alert (_ "parameter is nil."))
      nil)))
```
</details>

---

### ui:table

**说明**: 表格编辑,纯DCL方式实现,使用前先设置 ui:*table-title* 标题，赋值给 ui:*table-numbers-per-page* (整数)，可以设置每页的行数(默认为20)。ui:*table-widths* 用于定义每列的宽度，默认每列宽度为10。最大支持列数为26列。

**用法**:
```lisp
(ui:table lst-data)
```

**参数**: 1 lst-data  : 列表;

**返回值**: 修改后的数据

**示例**:
```lisp
(setq ui:*table-title* "我的表格")(setq ui:*table-widths* '(10 5 10 15))(ui:table '(("姓名"        "性别"        "年龄"        "身高")("张三"        "男"        18 180)("李四"        "女"        18 170)("王五"        "男"        18 180)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:table (lst-data / tmp-data dcl-tmp dcl_fp dcl_id pkg para% curr-page per-page tbl-widths tbl-title page-up page-down *error* show-data-list callback-accept)
  "表格编辑,纯DCL方式实现,使用前先设置 ui:*table-title* 标题，赋值给 ui:*table-numbers-per-page* (整数)，可以设置每页的行数(默认为20)。ui:*table-widths* 用于定义每列的宽度，默认每列宽度为10。最大支持列数为26列。"
  "修改后的数据"
  "(setq ui:*table-title* \"我的表格\")(setq ui:*table-widths* '(10 5 10 15))(ui:table '((\"姓名\"\n        \"性别\"\n        \"年龄\"\n        \"身高\")(\"张三\"\n        \"男\"\n        18 180)(\"李四\"\n        \"女\"\n        18 170)(\"王五\"\n        \"男\"\n        18 180)))"
  (defun *error* (msg)
    (if (= (quote file)
        (type dcl_fp))
      (close dcl_fp))
    (@:*error* msg))
  (if (and ui:*table-title* (= (quote str)
        (type ui:*table-title*)))
    (setq tbl-title ui:*table-title*)
    (setq tbl-title (_ "Table Editor")))
  (if (> (length (car lst-data))
      26)
    (progn (alert (_ "Max number of columns is 26."))
      (exit)))
  (if (and ui:*table-widths* (apply (quote and)
        (mapcar (quote numberp)
          ui:*table-widths*))
      (> (apply (quote min)
          ui:*table-widths*)
        0)
      (= (length ui:*table-widths*)
        (length (car lst-data))))
    (setq tbl-widths ui:*table-widths*)
    (progn (repeat (length (car lst-data))
        (setq tbl-widths (cons 10 tbl-widths)))))
  (if (and ui:*table-numbers-per-page* (= (quote int)
        (type ui:*table-numbers-per-page*)))
    (setq per-page ui:*table-numbers-per-page*)
    (setq per-page 20))
  (setq tmp-data lst-data)
  (defun save-table (/ lst-dcl)
    "save dcl data to lst-data"
    (setq lst-dcl (quote nil))
    (setq r% 0)
    (repeat (min per-page (- (length (cdr tmp-data))
          (* curr-page per-page)))
      (setq c% 64)
      (setq r% (1+ r%))
      (setq lst-r% (quote nil))
      (repeat (length (car tmp-data))
        (setq lst-r% (cons (get_tile (strcat (chr (setq c% (1+ c%)))
                (itoa r%)))
            lst-r%)))
      (setq lst-dcl (cons (reverse lst-r%)
          lst-dcl)))
    (setq lst-dcl (reverse lst-dcl))
    (setq i% 0)
    (setq tmp-data-prev (quote nil))
    (repeat (min (* curr-page per-page)
        (length (cdr tmp-data)))
      (setq tmp-data-prev (cons (nth (setq i% (1+ i%))
            tmp-data)
          tmp-data-prev)))
    (setq i% (* (1+ curr-page)
        per-page))
    (setq tmp-data-last (quote nil))
    (if (> (- (length (cdr tmp-data))
          (* (1+ curr-page)
            per-page))
        0)
      (repeat (- (length (cdr tmp-data))
          (* (1+ curr-page)
            per-page))
        (setq tmp-data-last (cons (nth (setq i% (1+ i%))
              tmp-data)
            tmp-data-last))))
    (setq tmp-data (append (list (car tmp-data))
        (reverse tmp-data-prev)
        lst-dcl (reverse tmp-data-last))))
  (defun callback-accept nil (save-table)
    (setq lst-data (mapcar (quote (lambda (x type1)
            (mapcar (quote (lambda (y ty)
                  (if (p:stringp y)
                    (@:to-type y (type ty))
                    y)))
              x type1)))
        tmp-data lst-data)))
  (defun page-up nil (save-table)
    (setq curr-page (1- curr-page))
    (set_tile "scrollbar"
      (itoa (- (1- total-page)
          curr-page)))
    (show-data-list))
  (defun page-down nil (save-table)
    (setq curr-page (1+ curr-page))
    (set_tile "scrollbar"
      (itoa (- (1- total-page)
          curr-page)))
    (show-data-list))
  (defun to-page nil (save-table)
    (setq curr-page (- (1- total-page)
        (atoi (get_tile "scrollbar"))))
    (show-data-list))
  (defun show-data-list (/ tmp-unit)
    (setq i% 0)
    (repeat (length (car tmp-data))
      (set_tile (strcat "header"
          (itoa (setq i% (1+ i%))))
        (nth (1- i%)
          (car tmp-data))))
    (setq c% 64)
    (repeat (length (car tmp-data))
      (setq r% 0 c% (1+ c%))
      (repeat (min per-page (- (length (cdr tmp-data))
            (* per-page curr-page)))
        (setq tmp-unit (nth (- c% 65)
            (nth (1- (+ (setq r% (1+ r%))
                  (* per-page curr-page)))
              (cdr tmp-data))))
        (set_tile (strcat (chr c%)
            (itoa r%))
          (cond ((p:intp tmp-unit)
              (string:number-format (@:to-string tmp-unit)
                5 0))
            ((numberp tmp-unit)
              (string:number-format (@:to-string tmp-unit)
                5 3))
            (t (@:to-string tmp-unit))))
        (mode_tile (strcat (chr c%)
            (itoa r%))
          0)
        (set_tile (strcat "rno"
            (itoa r%))
          (string:number-format (@:to-string (+ r% (* per-page curr-page)))
            7 0)))
      (while (< r% per-page)
        (repeat (length (car tmp-data))
          (set_tile (strcat (chr c%)
              (itoa (setq r% (1+ r%))))
            "")
          (mode_tile (strcat (chr c%)
              (itoa r%))
            1)
          (set_tile (strcat "rno"
              (itoa r%))
            ""))))
    (set_tile "curr_total"
      (strcat (itoa (1+ curr-page))
        "/"
        (itoa (1+ (/ (1- (length (cdr tmp-data)))
              per-page)))))
    (if (= 0 curr-page)
      (mode_tile "prev"
        1)
      (mode_tile "prev"
        0))
    (if (= (/ (1- (length (cdr tmp-data)))
          per-page)
        curr-page)
      (mode_tile "next"
        1)
      (mode_tile "next"
        0)))
  (if (/= (quote int)
      (type theme:bg-color))
    (setq theme:bg-color 151))
  (setq curr-page 0 total-page (fix (1+ (/ (1- (length (cdr tmp-data)))
          per-page))))
  (progn (setq dcl-tmp (strcat @:*tmp-path* "tmp-table-ed.dcl"))
    (setq dcl_fp (open dcl-tmp "w"))
    (write-line (strcat "table_ed : dialog {"
        "label = \""
        tbl-title "\";"
        ": column { "
        ":row{"
        ": column { ")
      dcl_fp)
    (write-line (strcat ": row { ")
      dcl_fp)
    (write-line (strcat ":edit_box{key=\"rno\";value=\"\n         行号\";width=2;fixed_width=true;horizontal_margin=none;vertical_margin=none;is_enabled=false;}")
      dcl_fp)
    (setq i% 0)
    (repeat (length (car tmp-data))
      (write-line (strcat ":edit_box{key=\"header"
          (itoa (setq i% (1+ i%)))
          "\";value=\"\";width="
          (itoa (nth (1- i%)
              tbl-widths))
          ";fixed_width=true;horizontal_margin=none;vertical_margin=none;is_enabled=false;}")
        dcl_fp))
    (write-line (strcat "}")
      dcl_fp)
    (setq r% 0)
    (repeat (min per-page (length (cdr tmp-data)))
      (write-line (strcat ":row{")
        dcl_fp)
      (setq r% (1+ r%)
        c% 64)
      (write-line (strcat ":edit_box{key=\"rno"
          (itoa r%)
          "\";value=\"\";width=2;fixed_width=true;horizontal_margin=none;vertical_margin=none;is_enabled=false;}")
        dcl_fp)
      (repeat (length (car tmp-data))
        (write-line (strcat ":edit_box{key=\""
            (chr (setq c% (1+ c%)))
            (itoa r%)
            "\";value=\"\";width="
            (itoa (nth (- c% 65)
                tbl-widths))
            ";fixed_width=true;horizontal_margin=none;vertical_margin=none;}")
          dcl_fp))
      (write-line (strcat "}")
        dcl_fp))
    (write-line (strcat "}")
      dcl_fp)
    (if (> (length (cdr tmp-data))
        per-page)
      (write-line (strcat ":slider{key=\"scrollbar\";layout=vertical;value="
          (itoa (1- total-page))
          ";max_value="
          (itoa (1- total-page))
          ";min_value=0;small_increment=1;}")
        dcl_fp))
    (write-line "}"
      dcl_fp)
    (if (> (length (cdr tmp-data))
        per-page)
      (progn (write-line ":spacer{}}:spacer{}"
          dcl_fp)
        (write-line ":row{alignment=centered;children_alignment=centered;:button{label=\"(<)Prev\";mnemonic=\"<\";key=\"prev\";is_enabled=false;}:spacer{} :text_part{key=\"curr_total\"; value=\"\";alignment=\"centered\";width=10;}:button{label=\"Next(>)\";mnemonic=\">\";key=\"next\";is_enabled=false;}}"
          dcl_fp))
      (write-line "}"
        dcl_fp))
    (write-line ":spacer{} ok_cancel; }"
      dcl_fp)
    (close dcl_fp))
  (setq dcl_id (load_dialog dcl-tmp))
  (if (not (new_dialog "table_ed"
        dcl_id))
    (exit))
  (action_tile "prev"
    "(page-up)")
  (action_tile "next"
    "(page-down)")
  (action_tile "scrollbar"
    "(to-page)")
  (action_tile "accept"
    "(callback-accept)(done_dialog 0)")
  (show-data-list)
  (start_dialog)
  (unload_dialog dcl_id)
  (vl-file-delete dcl-tmp)
  lst-data)
```
</details>

---

### ui:table-widths

**说明**: 求用于dcl表格的各列的自适应宽度。

**用法**:
```lisp
(ui:table-widths lst-data)
```

**参数**: 1 lst-data  : 列表;

**返回值**: 数值表

**示例**:
```lisp
(ui:table-widths '(("姓名"        "性别"        "年龄"        "身高")("张三""男"18 180)("李四"        "女"        18 170)("王五"        "男"        18 180)))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun ui:table-widths (lst-data)
  "求用于dcl表格的各列的自适应宽度。"
  "数值表"
  "(ui:table-widths '((\"姓名\"\n        \"性别\"\n        \"年龄\"\n        \"身高\")(\"张三\"\"男\"18 180)(\"李四\"\n        \"女\"\n        18 170)(\"王五\"\n        \"男\"\n        18 180)))"
  (mapcar (quote (lambda (x)
        (+ 2 (apply (quote max)
            (mapcar (quote (lambda (y)
                  (strbytelen (@:to-string y))))
              x)))))
    (apply (quote mapcar)
      (cons (quote list)
        lst-data))))
```
</details>

---

## vectra

**模块说明**: 专用功能模块

### vectra:acos

**用法**:
```lisp
(vectra:acos f)
```

**参数**: 1 f  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:acos (f)
  (if (equal f 0.0 1.0e-06)
    (* pi 0.5)
    (atan (/ (sqrt (- 1 (* f f)))
        f))))
```
</details>

---

### vectra:angle-include

**用法**:
```lisp
(vectra:angle-include a1 a2)
```

**参数**: 1 a1  : 未识别定义;2 a2  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:angle-include (a1 a2 / r)
  (setq r (abs (rem (- a1 a2)
        $2pi)))
  (if (> r pi)
    (- $2pi r)
    r))
```
</details>

---

### vectra:angle-normal

**用法**:
```lisp
(vectra:angle-normal a)
```

**参数**: 1 a  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:angle-normal (a)
  (setq a (rem a $2pi))
  (if (< a 0)
    (setq a (+ a $2pi)))
  a)
```
</details>

---

### vectra:angle-regular

**用法**:
```lisp
(vectra:angle-regular a)
```

**参数**: 1 a  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:angle-regular (a)
  (if (> a $pi/2)
    (- a pi)
    (if (< a (- $pi/2))
      (+ a pi)
      a)))
```
</details>

---

### vectra:angle-reverse

**用法**:
```lisp
(vectra:angle-reverse a)
```

**参数**: 1 a  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:angle-reverse (a)
  (rem (+ pi a)
    $2pi))
```
</details>

---

### vectra:array-create

**用法**:
```lisp
(vectra:array-create n)
```

**参数**: 1 n  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:array-create (n /)
  (vlax-make-safearray vlax-vbvariant (cons 0 n)))
```
</details>

---

### vectra:array-get

**用法**:
```lisp
(vectra:array-get arr n)
```

**参数**: 1 arr  : 未识别定义;2 n  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:array-get (arr n /)
  (vectra:var->list (vlax-safearray-get-element arr n)))
```
</details>

---

### vectra:array-set

**用法**:
```lisp
(vectra:array-set arr n value)
```

**参数**: 1 arr  : 未识别定义;2 n  : 未识别定义;3 value  : 值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:array-set (arr n value /)
  (vlax-safearray-put-element arr n (vectra:list->var value)))
```
</details>

---

### vectra:benchmark

**用法**:
```lisp
(vectra:benchmark expr n)
```

**参数**: 1 expr  : 表达式，一般指lisp表达式;2 n  : 整数;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:benchmark (expr n / elapse tps start)
  (setq start (getvar "MILLISECS"))
  (repeat n (eval expr))
  (setq elapse (- (getvar "MILLISECS")
      start))
  (if (equal elapse 0.0 1.0e-06)
    (strcat "Benchmark loops = "
      (itoa n)
      ", in "
      (rtos elapse 2 4)
      "
      ms")
    (strcat "Benchmark loops = "
      (itoa n)
      ", in "
      (rtos elapse 2 4)
      "
      ms, "
      (rtos (/ n elapse 0.001)
        2 0)
      "
      invoke / s")))
```
</details>

---

### vectra:block-items

**用法**:
```lisp
(vectra:block-items en)
```

**参数**: 1 en  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:block-items (en /)
  (setq $p-block-walked nil)
  (vectra:block-items-inner en))
```
</details>

---

### vectra:block-items-inner

**用法**:
```lisp
(vectra:block-items-inner en)
```

**参数**: 1 en  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:block-items-inner (en / name r)
  (if (= (quote str)
      (type en))
    (setq en (tblobjname "block"
        en)))
  (while (setq en (entnext en))
    (if (= "INSERT"
        (vectra:dxf en 0))
      (progn (setq name (vectra:dxf en 2))
        (if (not (member name $p-block-walked))
          (setq r (append r (vectra:block-items name))
            $p-block-walked (cons name $p-block-walked))))
      (setq r (cons en r))))
  r)
```
</details>

---

### vectra:block-trans

**用法**:
```lisp
(vectra:block-trans p geom)
```

**参数**: 1 p  : 未识别定义;2 geom  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:block-trans (p geom /)
  (mapcar (quote +)
    (vectra:mxv (car geom)
      p)
    (cadr geom)))
```
</details>

---

### vectra:ceiling

**用法**:
```lisp
(vectra:ceiling f dig)
```

**参数**: 1 f  : 未识别定义;2 dig  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:ceiling (f dig / num)
  (setq num (expt 10.0 dig))
  (/ (fix (+ 0.5 (* f num)))
    num))
```
</details>

---

### vectra:clipboard-get

**用法**:
```lisp
(vectra:clipboard-get )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:clipboard-get (/ html result)
  (and (setq html (vlax-create-object "htmlfile"))
    (setq result (vlax-invoke (vlax-get (vlax-get html (quote parentwindow))
          (quote clipboarddata))
        (quote getdata)
        "Text"))
    (vlax-release-object html))
  result)
```
</details>

---

### vectra:clipboard-set

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:clipboard-set (str / html result)
  (and (= (type str)
      (quote str))
    (setq html (vlax-create-object "htmlfile"))
    (setq result (vlax-invoke (vlax-get (vlax-get html (quote parentwindow))
          (quote clipboarddata))
        (quote setdata)
        "Text"
        str))
    (vlax-release-object html)))
```
</details>

---

### vectra:commandrun

**用法**:
```lisp
(vectra:commandrun func)
```

**参数**: 1 func  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:commandrun (func / *error*)
  (defun *error* (s)
    (vectra:error-handler s))
  (vectra:error-start0)
  (vectra:startundomark)
  (eval func)
  (vectra:endundomark)
  (vectra:error-end)
  (princ))
```
</details>

---

### vectra:commandrun-s

**用法**:
```lisp
(vectra:commandrun-s func sysvars)
```

**参数**: 1 func  : 未识别定义;2 sysvars  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:commandrun-s (func sysvars / *error*)
  (defun *error* (s)
    (vectra:error-handler s))
  (vectra:error-start sysvars)
  (vectra:startundomark)
  (eval func)
  (vectra:endundomark)
  (vectra:error-end)
  (princ))
```
</details>

---

### vectra:confirm

**用法**:
```lisp
(vectra:confirm msg default)
```

**参数**: 1 msg  : 提示信息;2 default  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:confirm (msg default / r)
  (initget "Y N ")
  (if (null (setq r (getkword (strcat msg "
            [是(Y)/否(N)] <"
            default ">:"))))
    (setq r default))
  r)
```
</details>

---

### vectra:csvfile-read

**用法**:
```lisp
(vectra:csvfile-read filename)
```

**参数**: 1 filename  : 文件名;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:csvfile-read (filename / r rows)
  (if (setq rows (vectra:file-read filename))
    (progn (foreach row rows (setq row (vl-string-trim "
            \t\n,"
            row))
        (if (and (/= row "")
            (/= (vectra:string-left row 2)
              "//")
            (setq row (vectra:string-tokenize row ",")))
          (setq row (mapcar (function (lambda (e)
                  (vl-string-subst "\""
                    "\"\"\""
                    e)))
              row)
            r (cons row r))))
      (reverse r))))
```
</details>

---

### vectra:csvfile-readcache

**用法**:
```lisp
(vectra:csvfile-readcache filename cache)
```

**参数**: 1 filename  : 文件名;2 cache  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:csvfile-readcache (filename cache / data)
  (if (and (setq filename (findfile filename))
      (setq filename (strcase filename))
      (null (setq data (vectra:get (vl-symbol-value cache)
            filename))))
    (progn (setq data (vectra:csvfile-read filename))
      (set cache (cons (cons filename data)
          (vl-symbol-value cache)))))
  data)
```
</details>

---

### vectra:csvread-get

**用法**:
```lisp
(vectra:csvread-get csv key)
```

**参数**: 1 csv  : 未识别定义;2 key  : 键，关键字;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:csvread-get (csv key /)
  (mapcar (function cons)
    (car csv)
    (vectra:get1 (cdr csv)
      key)))
```
</details>

---

### vectra:csvread-get1

**用法**:
```lisp
(vectra:csvread-get1 csv key)
```

**参数**: 1 csv  : 未识别定义;2 key  : 键，关键字;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:csvread-get1 (csv key / r)
  (setq r (vectra:csvread-get csv key))
  (mapcar (function (lambda (e)
        (if (= "#"
            (vectra:string-left (car e)
              1))
          (cons (vl-string-trim "#"
              (car e))
            (atof (cdr e)))
          e)))
    r))
```
</details>

---

### vectra:csvread-keys

**用法**:
```lisp
(vectra:csvread-keys csv)
```

**参数**: 1 csv  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:csvread-keys (csv /)
  (mapcar (function car)
    (cdr csv)))
```
</details>

---

### vectra:deg->rad

**用法**:
```lisp
(vectra:deg->rad deg)
```

**参数**: 1 deg  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:deg->rad (deg)
  (* pi (/ deg 180.0)))
```
</details>

---

### vectra:directory-make

**用法**:
```lisp
(vectra:directory-make folder)
```

**参数**: 1 folder  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:directory-make (folder / folders cur)
  (setq folder (vl-string-trim "\\/"
      folder)
    folders (vectra:string-tokenize folder "\\/")
    cur (car folders)
    folders (cdr folders))
  (foreach e folders (setq cur (strcat cur "\\"
        E))
    (IF (NULL (VL-FILE-DIRECTORY-P CUR))
      (VL-MKDIR CUR))))
```
</details>

---

### vectra:dxf

**用法**:
```lisp
(vectra:dxf ename keys)
```

**参数**: 1 ename  : 单个图元;2 keys  : 键，关键字;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:dxf (ename keys /)
  (vectra:get (entget ename)
    keys))
```
</details>

---

### vectra:dxf1

**用法**:
```lisp
(vectra:dxf1 ename keys)
```

**参数**: 1 ename  : 单个图元;2 keys  : 键，关键字;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:dxf1 (ename keys /)
  (vectra:get1 (entget ename)
    keys))
```
</details>

---

### vectra:dxfs

**用法**:
```lisp
(vectra:dxfs ename sym keys)
```

**参数**: 1 ename  : 单个图元;2 sym  : 未识别定义;3 keys  : 键，关键字;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:dxfs (ename sym keys /)
  (mapcar (function set)
    sym (vectra:dxf ename keys)))
```
</details>

---

### vectra:edit-value

**用法**:
```lisp
(vectra:edit-value msg old)
```

**参数**: 1 msg  : 提示信息;2 old  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:edit-value (msg old / value)
  (cond ((= (quote real)
        (type old))
      (setq value (getdist (strcat msg "
            <"
            (rtos old 2 2)
            ">: "))))
    ((= (quote int)
        (type old))
      (setq value (getint (strcat msg "
            <"
            (itoa old)
            ">: "))))
    ((= (quote list)
        (type old))
      (setq value (getpoint (strcat msg "
            <"
            (vl-princ-to-string old)
            ">: "))))
    ((= (quote str)
        (type old))
      (setq value (getstring (strcat msg "
            <"
            old ">: ")))))
  (if (or (null value)
      (= ""
        value))
    old value))
```
</details>

---

### vectra:enamep

**用法**:
```lisp
(vectra:enamep v)
```

**参数**: 1 v  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:enamep (v)
  (= (quote ename)
    (type v)))
```
</details>

---

### vectra:enames->ss

**用法**:
```lisp
(vectra:enames->ss enames)
```

**参数**: 1 enames  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:enames->ss (enames / ss)
  (setq ss (ssadd))
  (foreach en enames (ssadd en ss))
  ss)
```
</details>

---

### vectra:enames-after

**用法**:
```lisp
(vectra:enames-after en ss)
```

**参数**: 1 en  : 单个图元;2 ss  : 选择集;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:enames-after (en ss /)
  (if (null ss)
    (setq ss (ssadd)))
  (while (setq en (entnext en))
    (setq ss (ssadd en ss)))
  ss)
```
</details>

---

### vectra:endundomark

**用法**:
```lisp
(vectra:endundomark )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:endundomark nil (if $p-undo-started (progn (vla-endundomark (vla-get-activedocument (vlax-get-acad-object)))
      (setq $p-undo-started nil))))
```
</details>

---

### vectra:ensure-ename

**用法**:
```lisp
(vectra:ensure-ename obj)
```

**参数**: 1 obj  : activeX 对象;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:ensure-ename (obj)
  (if (/= (quote ename)
      (type obj))
    (vlax-vla-object->ename obj)
    obj))
```
</details>

---

### vectra:ensure-object

**用法**:
```lisp
(vectra:ensure-object obj)
```

**参数**: 1 obj  : activeX 对象;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:ensure-object (obj)
  (if (= (quote ename)
      (type obj))
    (vlax-ename->vla-object obj)
    obj))
```
</details>

---

### vectra:entmake

**用法**:
```lisp
(vectra:entmake dxf)
```

**参数**: 1 dxf  : 组码号;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:entmake (dxf / r)
  (vectra:linetype-get $addnew-linetype)
  (vl-catch-all-apply (quote entmakex)
    (list (append dxf (list (cons 8 $addnew-layer)
          (cons 6 $addnew-linetype)
          (cons 62 $addnew-color)
          (cons 370 $addnew-lineweight)))))
  (setq r (entlast))
  (if (null (vl-catch-all-error-p r))
    (if $addnew-xdata (vectra:xdata-set r (car $addnew-xdata)
        (cdr $addnew-xdata))))
  r)
```
</details>

---

### vectra:entmod

**用法**:
```lisp
(vectra:entmod ent datas)
```

**参数**: 1 ent  : 单个图元;2 datas  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:entmod (ent datas /)
  (entmod (vectra:set (entget ent)
      datas)))
```
</details>

---

### vectra:entsel

**用法**:
```lisp
(vectra:entsel msg filter)
```

**参数**: 1 msg  : 提示信息;2 filter  : 过滤dxf组码;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:entsel (msg filter /)
  (vectra:entsel-inner msg filter nil))
```
</details>

---

### vectra:entsel-inner

**用法**:
```lisp
(vectra:entsel-inner msg filter nested)
```

**参数**: 1 msg  : 提示信息;2 filter  : 过滤dxf组码;3 nested  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:entsel-inner (msg filter nested / ent kword)
  (if (and msg (setq kword (mapcar (quote car)
          (vl-remove-if-not (quote vl-consp)
            (vectra:template-parse-inner msg "("
                ")")))))
    (setq kword (vectra:string-connect kword "
        ")
      kword (strcat kword "
         "))
    (setq kword "
       "))
  (while (null ent)
    (initget kword)
    (if nested (setq ent (nentsel msg))
      (setq ent (entsel msg)))
    (cond ((null ent)
        (princ "未选择对象。"))
      ((= (type ent)
          (quote list))
        (if (and filter (not (wcmatch (vectra:dxf (car ent)
                  0)
                filter)))
          (progn (princ "选择对象已被过滤。")
            (setq ent nil))))))
  ent)
```
</details>

---

### vectra:error-end

**用法**:
```lisp
(vectra:error-end )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:error-end nil (if $p-sysvars-saved (progn (vectra:setvars $p-sysvars-saved)
      (setq $p-sysvars-saved nil))))
```
</details>

---

### vectra:error-handler

**用法**:
```lisp
(vectra:error-handler s)
```

**参数**: 1 s  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:error-handler (s)
  (if (or (= s "Function cancelled")
      (= s "quit / exit abort")
      (= s "函数被取消"))
    (princ)
    (princ s))
  (while (not (equal (getvar "CMDNAMES")
        ""))
    (command nil))
  (vectra:osnap-restore)
  (vectra:endundomark)
  (vectra:error-end)
  (princ))
```
</details>

---

### vectra:error-start

**用法**:
```lisp
(vectra:error-start vars)
```

**参数**: 1 vars  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:error-start (vars /)
  (setq $p-sysvars-saved (vectra:setvars vars)))
```
</details>

---

### vectra:error-start0

**用法**:
```lisp
(vectra:error-start0 )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:error-start0 (/)
  (vectra:error-start (quote (("CMDECHO"
          . 0)))))
```
</details>

---

### vectra:file-read

**用法**:
```lisp
(vectra:file-read filename)
```

**参数**: 1 filename  : 文件名;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:file-read (filename / content file line)
  (if (and (setq filename (findfile filename))
      (setq file (open filename "r")))
    (progn (while (setq line (read-line file))
        (setq content (cons line content)))
      (close file)))
  (reverse content))
```
</details>

---

### vectra:file-readstring

**用法**:
```lisp
(vectra:file-readstring filename)
```

**参数**: 1 filename  : 文件名;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:file-readstring (filename / content)
  (if (setq content (vectra:file-read filename))
    (vectra:string-connect content "\r\n")))
```
</details>

---

### vectra:file-search

**用法**:
```lisp
(vectra:file-search path patten)
```

**参数**: 1 path  : 文件路径;2 patten  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:file-search (path patten / r)
  (foreach dir (cddr (vl-directory-files path nil -1))
    (setq r (append r (vectra:file-search (strcat path "\\"
            DIR)
          PATTEN))))
  (FOREACH FILE (VL-DIRECTORY-FILES PATH PATTEN 0)
    (SETQ R (CONS (STRCAT PATH "\\"
          file)
        r)))
  r)
```
</details>

---

### vectra:get

**用法**:
```lisp
(vectra:get lst keys)
```

**参数**: 1 lst  : 列表;2 keys  : 键，关键字;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:get (lst keys /)
  (if (atom keys)
    (cdr (assoc keys lst))
    (mapcar (function (lambda (e)
          (cdr (assoc e lst))))
      keys)))
```
</details>

---

### vectra:get-block

**用法**:
```lisp
(vectra:get-block name)
```

**参数**: 1 name  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:get-block (name /)
  (vl-catch-all-apply (quote vla-item)
    (list (vla-get-blocks (vla-get-activedocument (vlax-get-acad-object)))
      name)))
```
</details>

---

### vectra:get-bulge

**用法**:
```lisp
(vectra:get-bulge v1 v2)
```

**参数**: 1 v1  : 未识别定义;2 v2  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:get-bulge (v1 v2 / a1 a2 bulge dot)
  (setq dot (sp-dotproduct v1 v2)
    a1 (sp-angle v1)
    a2 (sp-angle v2))
  (if (< dot 0)
    (setq bulge (tan (/ (acos (abs dot))
          4.0)))
    (setq bulge (tan (/ (- pi (acos (abs dot)))
          4.0))))
  (setq a2 (- a2 a1))
  (if (or (and (>= a2 0)
        (< a2 pi))
      (and (< a2 0)
        (< a2 (- pi))))
    (setq bulge (- bulge)))
  bulge)
```
</details>

---

### vectra:get1

**用法**:
```lisp
(vectra:get1 lst keys)
```

**参数**: 1 lst  : 列表;2 keys  : 键，关键字;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:get1 (lst keys / e)
  (if (atom keys)
    (cons keys (vectra:get lst keys))
    (mapcar (quote cons)
      keys (vectra:get lst keys))))
```
</details>

---

### vectra:getdist

**用法**:
```lisp
(vectra:getdist msg old p)
```

**参数**: 1 msg  : 提示信息;2 old  : 未识别定义;3 p  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:getdist (msg old p / value)
  (if p (setq value (getdist (strcat msg "
          <"
          (rtos old 2 2)
          ">: ")
        p))
    (setq value (getdist (strcat msg "
          <"
          (rtos old 2 2)
          ">: "))))
  (if (null value)
    old value))
```
</details>

---

### vectra:getint

**用法**:
```lisp
(vectra:getint msg init default)
```

**参数**: 1 msg  : 提示信息;2 init  : 未识别定义;3 default  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:getint (msg init default / r)
  (if init (initget init))
  (if (null (setq r (getint (strcat msg "
            <"
            (itoa default)
            ">"))))
    default r))
```
</details>

---

### vectra:getkword

**用法**:
```lisp
(vectra:getkword msg kword default)
```

**参数**: 1 msg  : 提示信息;2 kword  : 未识别定义;3 default  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:getkword (msg kword default / r)
  (initget kword)
  (if (null (setq r (getkword (strcat msg "
            <"
            default ">: "))))
    default r))
```
</details>

---

### vectra:getkword1

**用法**:
```lisp
(vectra:getkword1 msg kwords default)
```

**参数**: 1 msg  : 提示信息;2 kwords  : 未识别定义;3 default  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:getkword1 (msg kwords default / r)
  (initget (vectra:string-connect (mapcar (quote car)
        kwords)
      "
      "))
  (setq r (getkword (strcat msg "
        ["
        (vectra:string-connect (mapcar (function (lambda (e)
                (strcat (cadr e)
                  "("
                    (car e)
                    ")")))
            kwords)
          "/")
        "] <"
        default ">: ")))
  (if (null r)
    default r))
```
</details>

---

### vectra:hash

**用法**:
```lisp
(vectra:hash lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:hash (lst /)
  (setq h (getvar (quote millisecs)))
  (if (atom lst)
    (setq h (vectra:hash-1 lst h))
    (foreach e lst (setq h (vectra:hash-1 e h)))))
```
</details>

---

### vectra:hash-1

**用法**:
```lisp
(vectra:hash-1 e h)
```

**参数**: 1 e  : 未识别定义;2 h  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:hash-1 (e h /)
  (+ (fix e)
    (* 31 h)))
```
</details>

---

### vectra:insert-seqs

**用法**:
```lisp
(vectra:insert-seqs en)
```

**参数**: 1 en  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:insert-seqs (en / r)
  (while (and (setq en (entnext en))
      (/= "SEQEND"
        (vectra:dxf en 0)))
    (setq r (cons en r)))
  r)
```
</details>

---

### vectra:item

**用法**:
```lisp
(vectra:item obj name)
```

**参数**: 1 obj  : activeX 对象;2 name  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:item (obj name / r)
  (if (vl-catch-all-error-p (setq r (vl-catch-all-apply (quote vla-item)
          (list obj name))))
    nil r))
```
</details>

---

### vectra:jscript-eval

**用法**:
```lisp
(vectra:jscript-eval func_str)
```

**参数**: 1 func_str  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:jscript-eval (func_str /)
  (if (null $p-scriptcontrol)
    (progn (setq $p-scriptcontrol (vlax-create-object "Aec32BitAppServer.AecScriptControl.1"))
      (vl-catch-all-apply (quote vlax-put)
        (list $p-scriptcontrol "language"
          "jscript"))))
  (if (null $p-scriptcontrol)
    nil (vl-catch-all-apply (quote vlax-invoke)
      (list $p-scriptcontrol "eval"
        func_str))))
```
</details>

---

### vectra:layer-get

**用法**:
```lisp
(vectra:layer-get name conf)
```

**参数**: 1 name  : 未识别定义;2 conf  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:layer-get (name conf / dxf layer)
  (if (null (setq layer (tblsearch "LAYER"
          name)))
    (progn (setq dxf (list (quote (0 . "LAYER"))
          (quote (100 . "AcDbSymbolTableRecord"))
          (quote (100 . "AcDbLayerTableRecord"))
          (quote (70 . 0))
          (quote (6 . "Continuous"))
          (cons 2 name)))
      (if (car conf)
        (setq dxf (append dxf (list (cons 62 (atoi (car conf)))))))
      (if (cadr conf)
        (progn (vectra:linetype-get (cadr conf))
          (setq dxf (append dxf (list (cons 6 (cadr conf)))))))
      (if (caddr conf)
        (setq dxf (append dxf (list (cons 370 (atoi (caddr conf)))))))
      (if (and (not (null (cadddr conf)))
          (= "FALSE"
            (strcase (cadddr conf))))
        (setq dxf (append dxf (list (cons 290 0)))))
      (entmake dxf))
    layer))
```
</details>

---

### vectra:layer-get1

**用法**:
```lisp
(vectra:layer-get1 name dxf)
```

**参数**: 1 name  : 未识别定义;2 dxf  : 组码号;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:layer-get1 (name dxf / r)
  (setq r (tblsearch "LAYER"
      name))
  (if (null r)
    (setq r (entmake (vectra:set (list (quote (0 . "LAYER"))
            (quote (100 . "AcDbSymbolTableRecord"))
            (quote (100 . "AcDbLayerTableRecord"))
            (quote (70 . 0))
            (quote (6 . "Continuous"))
            (quote (62 . 7))
            (quote (370 . -3))
            (cons 2 name))
          dxf))))
  r)
```
</details>

---

### vectra:line-closestpoint

**用法**:
```lisp
(vectra:line-closestpoint line p extend)
```

**参数**: 1 line  : 未识别定义;2 p  : 未识别定义;3 extend  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:line-closestpoint (line p extend)
  (vlax-curve-getclosestpointto (vectra:ensure-object line)
    p extend))
```
</details>

---

### vectra:line-getangle

**用法**:
```lisp
(vectra:line-getangle line)
```

**参数**: 1 line  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:line-getangle (line /)
  (apply (quote angle)
    (vectra:dxf (vectra:ensure-ename line)
      (quote (10 11)))))
```
</details>

---

### vectra:line-getendnear

**用法**:
```lisp
(vectra:line-getendnear line p)
```

**参数**: 1 line  : 未识别定义;2 p  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:line-getendnear (line p / pts)
  (setq pts (vectra:dxf1 line (quote (10 11))))
  (if (< (distance (cdar pts)
        p)
      (distance (cdadr pts)
        p))
    (car pts)
    (cadr pts)))
```
</details>

---

### vectra:line-getinters

**用法**:
```lisp
(vectra:line-getinters l1 l2)
```

**参数**: 1 l1  : 未识别定义;2 l2  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:line-getinters (l1 l2 / dx1 dx2)
  (setq dx1 (entget l1)
    dx2 (entget l2))
  (inters (cdr (assoc 10 dx1))
    (cdr (assoc 11 dx1))
    (cdr (assoc 10 dx2))
    (cdr (assoc 11 dx2))
    nil))
```
</details>

---

### vectra:line-parallel

**用法**:
```lisp
(vectra:line-parallel l1 l2)
```

**参数**: 1 l1  : 未识别定义;2 l2  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:line-parallel (l1 l2 / a)
  (setq a (vectra:angle-include (vectra:line-getangle l1)
      (vectra:line-getangle l2)))
  (or (equal a pi 0.01)
    (equal a 0.0 0.01)))
```
</details>

---

### vectra:linetype-get

**用法**:
```lisp
(vectra:linetype-get name)
```

**参数**: 1 name  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:linetype-get (name / lt)
  (setq lt (vl-catch-all-apply (quote vla-item)
      (list (vla-get-linetypes (vla-get-activedocument (vlax-get-acad-object)))
        name)))
  (if (vl-catch-all-error-p lt)
    (setq lt (vectra:linetype-load name "acad.lin")))
  lt)
```
</details>

---

### vectra:linetype-load

**用法**:
```lisp
(vectra:linetype-load name filename)
```

**参数**: 1 name  : 未识别定义;2 filename  : 文件名;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:linetype-load (name filename /)
  (vla-load (vla-get-linetypes (vla-get-activedocument (vlax-get-acad-object)))
    name filename))
```
</details>

---

### vectra:lisp-load

**用法**:
```lisp
(vectra:lisp-load filename)
```

**参数**: 1 filename  : 文件名;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:lisp-load (filename /)
  (if (setq content (vectra:file-readstring filename))
    (read content)
    (princ (strcat "\npsk-load-lispfile错误: 无法加载文件 \""
        filename "\""))))
```
</details>

---

### vectra:list->var

**用法**:
```lisp
(vectra:list->var lsp)
```

**参数**: 1 lsp  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:list->var (lsp / e n var)
  (if (vl-consp lsp)
    (progn (setq n (1- (length lsp))
        var (vlax-make-safearray vlax-vbvariant (cons 0 n)))
      (vlax-safearray-fill var (mapcar (quote (lambda (e)
              (vectra:list->var e)))
          lsp))
      var)
    (vlax-make-variant lsp)))
```
</details>

---

### vectra:make-arc

**用法**:
```lisp
(vectra:make-arc point radius start end)
```

**参数**: 1 point  : 未识别定义;2 radius  : 未识别定义;3 start  : 未识别定义;4 end  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:make-arc (point radius start end /)
  (vectra:entmake (list (quote (0 . "ARC"))
      (cons 10 point)
      (cons 40 radius)
      (cons 50 start)
      (cons 51 end))))
```
</details>

---

### vectra:make-block

**用法**:
```lisp
(vectra:make-block name funcname params)
```

**参数**: 1 name  : 未识别定义;2 funcname  : 未识别定义;3 params  : 参数列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:make-block (name funcname params / en r)
  (entmake (list (cons 0 "BLOCK")
      (cons 2 name)
      (cons 70 (if (= "*U"
            name)
          1 0))
      (cons 10 $addnew-block-base)))
  (setq r (vl-catch-all-apply funcname params))
  (setq en (entmake (quote ((0 . "ENDBLK")))))
  (if (vl-catch-all-error-p r)
    (progn (princ (strcat "\nerror in vectra:make-block:"
          (vl-princ-to-string (cons funcname params))))
      nil)
    en))
```
</details>

---

### vectra:make-circle

**用法**:
```lisp
(vectra:make-circle center radius)
```

**参数**: 1 center  : 未识别定义;2 radius  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:make-circle (center radius /)
  (vectra:entmake (list (quote (0 . "CIRCLE"))
      (cons 10 center)
      (cons 40 radius))))
```
</details>

---

### vectra:make-ellipse

**用法**:
```lisp
(vectra:make-ellipse p1 p2 r1)
```

**参数**: 1 p1  : 未识别定义;2 p2  : 未识别定义;3 r1  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:make-ellipse (p1 p2 r1)
  (vectra:entmake (list (quote (0 . "ELLIPSE"))
      (quote (100 . "AcDbEntity"))
      (quote (100 . "AcDbEllipse"))
      (cons 10 p1)
      (cons 11 p2)
      (cons 40 r1)
      (cons 41 0.0)
      (quote (42 . 6.28319)))))
```
</details>

---

### vectra:make-insert

**用法**:
```lisp
(vectra:make-insert name point sx sy sz ang)
```

**参数**: 1 name  : 未识别定义;2 point  : 未识别定义;3 sx  : 未识别定义;4 sy  : 未识别定义;5 sz  : 未识别定义;6 ang  : 角度值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:make-insert (name point sx sy sz ang)
  (vectra:entmake (list (quote (0 . "INSERT"))
      (cons 2 name)
      (cons 10 point)
      (cons 41 sx)
      (cons 42 sy)
      (cons 43 sz)
      (cons 50 ang))))
```
</details>

---

### vectra:make-insert-with-funcs

**用法**:
```lisp
(vectra:make-insert-with-funcs name point funcname params)
```

**参数**: 1 name  : 未识别定义;2 point  : 未识别定义;3 funcname  : 未识别定义;4 params  : 参数列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:make-insert-with-funcs (name point funcname params /)
  (setq name (vectra:make-block name funcname params))
  (vectra:make-insert name point 1.0 1.0 1.0 0.0))
```
</details>

---

### vectra:make-insert-with-funcs-a

**用法**:
```lisp
(vectra:make-insert-with-funcs-a name point funcname params sx sy sz ang)
```

**参数**: 1 name  : 未识别定义;2 point  : 未识别定义;3 funcname  : 未识别定义;4 params  : 参数列表;5 sx  : 未识别定义;6 sy  : 未识别定义;7 sz  : 未识别定义;8 ang  : 角度值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:make-insert-with-funcs-a (name point funcname params sx sy sz ang /)
  (setq name (vectra:make-block name funcname params))
  (vectra:make-insert name point sx sy sz ang))
```
</details>

---

### vectra:make-line

**用法**:
```lisp
(vectra:make-line p1 p2)
```

**参数**: 1 p1  : 未识别定义;2 p2  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:make-line (p1 p2 /)
  (vectra:entmake (list (quote (0 . "LINE"))
      (cons 10 p1)
      (cons 11 p2))))
```
</details>

---

### vectra:make-polyline

**用法**:
```lisp
(vectra:make-polyline points closed width)
```

**参数**: 1 points  : 未识别定义;2 closed  : 未识别定义;3 width  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:make-polyline (points closed width / e)
  (vectra:entmake (append (list (quote (0 . "LWPOLYLINE"))
        (quote (100 . "AcDbEntity"))
        (quote (100 . "AcDbPolyline"))
        (cons 90 (length points))
        (cons 70 closed)
        (cons 43 width))
      (mapcar (quote (lambda (e)
            (cons 10 e)))
        points))))
```
</details>

---

### vectra:make-setenv

**用法**:
```lisp
(vectra:make-setenv vars)
```

**参数**: 1 vars  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:make-setenv (vars / rv)
  (setq rv (list $addnew-layer $addnew-color $addnew-linetype $addnew-lineweight $addnew-textstyle $addnew-block-base))
  (vectra:set-symbol-notnull (quote $addnew-layer)
    (car vars))
  (vectra:set-symbol-notnull (quote $addnew-color)
    (cadr vars))
  (vectra:set-symbol-notnull (quote $addnew-linetype)
    (caddr vars))
  (vectra:set-symbol-notnull (quote $addnew-lineweight)
    (cadddr vars))
  (vectra:set-symbol-notnull (quote $addnew-textstyle)
    (car (cddddr vars)))
  (vectra:set-symbol-notnull (quote $addnew-block-base)
    (cadr (cddddr vars)))
  rv)
```
</details>

---

### vectra:make-sharparc

**用法**:
```lisp
(vectra:make-sharparc p r a4 a5)
```

**参数**: 1 p  : 未识别定义;2 r  : 未识别定义;3 a4  : 未识别定义;4 a5  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:make-sharparc (p r a4 a5)
  (if (or (and (> a5 a4)
        (< (- a5 a4)
          pi))
      (and (< a5 a4)
        (> (- a4 a5)
          pi)))
    (vectra:make-arc p r a4 a5)
    (vectra:make-arc p r a5 a4)))
```
</details>

---

### vectra:make-text

**用法**:
```lisp
(vectra:make-text text point align height width ang)
```

**参数**: 1 text  : 未识别定义;2 point  : 未识别定义;3 align  : 未识别定义;4 height  : 未识别定义;5 width  : 未识别定义;6 ang  : 角度值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:make-text (text point align height width ang / c)
  (vectra:textstyle-get $addnew-textstyle "gbenor.shx"
    "hztxt.shx"
    0.7)
  (setq c (cdr (assoc (strcase align)
        (quote (("L"
              0 0)
            ("C"
              1 0)
            ("R"
              2 0)
            ("A"
              3 0)
            ("M"
              4 3)
            ("F"
              5 0)
            ("TL"
              0 3)
            ("TC"
              1 3)
            ("TR"
              2 3)
            ("ML"
              0 2)
            ("MC"
              1 2)
            ("MR"
              2 2)
            ("BL"
              0 1)
            ("BC"
              1 1)
            ("BR"
              2 1))))))
  (vectra:entmake (list (quote (0 . "TEXT"))
      (cons 1 text)
      (cons 7 $addnew-textstyle)
      (cons 10 point)
      (cons 11 point)
      (cons 40 height)
      (cons 41 width)
      (cons 50 ang)
      (cons 72 (car c))
      (cons 73 (cadr c)))))
```
</details>

---

### vectra:mid

**用法**:
```lisp
(vectra:mid p1 p2)
```

**参数**: 1 p1  : 未识别定义;2 p2  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:mid (p1 p2 / e)
  (mapcar (function (lambda (e)
        (/ e 2.0)))
    (mapcar (function +)
      p1 p2)))
```
</details>

---

### vectra:mxm

**用法**:
```lisp
(vectra:mxm m q)
```

**参数**: 1 m  : 未识别定义;2 q  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:mxm (m q)
  (mapcar (quote (lambda (r)
        (vectra:mxv (vectra:trp q)
          r)))
    m))
```
</details>

---

### vectra:mxv

**用法**:
```lisp
(vectra:mxv m v)
```

**参数**: 1 m  : 未识别定义;2 v  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:mxv (m v)
  (mapcar (quote (lambda (r)
        (apply (quote +)
          (mapcar (quote *)
            r v))))
    m))
```
</details>

---

### vectra:nentsel

**用法**:
```lisp
(vectra:nentsel msg filter)
```

**参数**: 1 msg  : 提示信息;2 filter  : 过滤dxf组码;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:nentsel (msg filter /)
  (vectra:entsel-inner msg filter t))
```
</details>

---

### vectra:number-padding

**用法**:
```lisp
(vectra:number-padding number len)
```

**参数**: 1 number  : 未识别定义;2 len  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:number-padding (number len / r)
  (setq r (itoa number))
  (repeat (- len (strlen r))
    (setq r (strcat "0"
        r)))
  r)
```
</details>

---

### vectra:number-padding-last

**用法**:
```lisp
(vectra:number-padding-last number len)
```

**参数**: 1 number  : 未识别定义;2 len  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:number-padding-last (number len / p r)
  (setq r (rtos number 2 len))
  (if (/= len 0)
    (progn (setq p (vl-string-search "."
          r))
      (if (null p)
        (setq p (strlen r)
          r (strcat r ".")))
      (repeat (- len (strlen r)
          (- p)
          -1)
        (setq r (strcat r "0")))))
  r)
```
</details>

---

### vectra:osnap-disable

**用法**:
```lisp
(vectra:osnap-disable )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:osnap-disable nil (if (null $p-saved-osmode)
    (setq $p-saved-osmode (getvar "OSMODE")))
  (if (< $p-saved-osmode 16384)
    (setvar "OSMODE"
      (+ $p-saved-osmode 16384))))
```
</details>

---

### vectra:osnap-restore

**用法**:
```lisp
(vectra:osnap-restore )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:osnap-restore nil (if (not (null $p-saved-osmode))
    (progn (setvar "OSMODE"
        $p-saved-osmode)
      (setq $p-saved-osmode nil))))
```
</details>

---

### vectra:rad->deg

**用法**:
```lisp
(vectra:rad->deg rad)
```

**参数**: 1 rad  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:rad->deg (rad)
  (* 180.0 (/ rad pi)))
```
</details>

---

### vectra:readme

**说明**: 来源于明经大佬 vectra 开源的 152个函数.

**用法**:
```lisp
(vectra:readme )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:readme nil "来源于明经大佬 vectra 开源的 152个函数.")
```
</details>

---

### vectra:refgeom

**用法**:
```lisp
(vectra:refgeom ename)
```

**参数**: 1 ename  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:refgeom (ename / elst ang norm mat)
  (setq elst (entget ename)
    ang (cdr (assoc 50 elst))
    norm (cdr (assoc 210 elst)))
  (list (setq mat (vectra:mxm (mapcar (function (lambda (v)
              (trans v 0 norm t)))
          (quote ((1.0 0.0 0.0)
              (0.0 1.0 0.0)
              (0.0 0.0 1.0))))
        (vectra:mxm (list (list (cos ang)
              (- (sin ang))
              0.0)
            (list (sin ang)
              (cos ang)
              0.0)
            (quote (0.0 0.0 1.0)))
          (list (list (cdr (assoc 41 elst))
              0.0 0.0)
            (list 0.0 (cdr (assoc 42 elst))
              0.0)
            (list 0.0 0.0 (cdr (assoc 43 elst)))))))
    (mapcar (quote -)
      (trans (cdr (assoc 10 elst))
        norm 0)
      (vectra:mxv mat (cdr (assoc 10 (tblsearch "BLOCK"
              (cdr (assoc 2 elst)))))))))
```
</details>

---

### vectra:regexp-match

**用法**:
```lisp
(vectra:regexp-match str pattern)
```

**参数**: 1 str  : 字符串;2 pattern  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:regexp-match (str pattern / matchs reg r)
  (setq reg (vlax-create-object "vbscript.regexp"))
  (vlax-put-property reg (quote global)
    1)
  (vlax-put-property reg (quote ignorecase)
    1)
  (vlax-put-property reg (quote pattern)
    pattern)
  (setq matchs (vlax-invoke reg (quote execute)
      str))
  (vlax-for k matchs (setq r (cons (vlax-get k (quote value))
        r)))
  (vlax-release-object reg)
  (reverse r))
```
</details>

---

### vectra:regexp-replace

**用法**:
```lisp
(vectra:regexp-replace str pattern new)
```

**参数**: 1 str  : 字符串;2 pattern  : 未识别定义;3 new  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:regexp-replace (str pattern new / reg r)
  (setq reg (vlax-create-object "vbscript.regexp"))
  (vlax-put-property reg (quote pattern)
    pattern)
  (vlax-put-property reg (quote global)
    1)
  (vlax-put-property reg (quote ignorecase)
    1)
  (setq r (vlax-invoke reg (quote replace)
      str new))
  (vlax-release-object reg)
  r)
```
</details>

---

### vectra:round

**用法**:
```lisp
(vectra:round f num)
```

**参数**: 1 f  : 未识别定义;2 num  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:round (f num /)
  (fix (- f (rem f num)
      (- num))))
```
</details>

---

### vectra:set

**用法**:
```lisp
(vectra:set lst values)
```

**参数**: 1 lst  : 列表;2 values  : 值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:set (lst values / old)
  (if (and values (atom (car values)))
    (setq values (list values)))
  (foreach value values (if (setq old (assoc (car value)
          lst))
      (setq lst (subst value old lst))
      (setq lst (append lst (list value)))))
  lst)
```
</details>

---

### vectra:set-symbol-notnull

**用法**:
```lisp
(vectra:set-symbol-notnull sym val)
```

**参数**: 1 sym  : 未识别定义;2 val  : 值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:set-symbol-notnull (sym val /)
  (if val (set sym val)))
```
</details>

---

### vectra:set-values

**用法**:
```lisp
(vectra:set-values lst)
```

**参数**: 1 lst  : 列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:set-values (lst / old)
  (foreach e lst (setq old (cons (cons (car e)
          (vl-symbol-value (car e)))
        old))
    (set (car e)
      (cdr e)))
  (reverse old))
```
</details>

---

### vectra:set1

**用法**:
```lisp
(vectra:set1 lst key value)
```

**参数**: 1 lst  : 列表;2 key  : 键，关键字;3 value  : 值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:set1 (lst key value / old)
  (if (setq old (assoc key lst))
    (setq lst (subst (cons key value)
        old lst))
    (setq lst (append lst (list (cons key value))))))
```
</details>

---

### vectra:setvars

**用法**:
```lisp
(vectra:setvars pairs)
```

**参数**: 1 pairs  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:setvars (pairs / changed name old value)
  (if (atom (car pairs))
    (setq pairs (list pairs)))
  (foreach pair pairs (setq name (car pair)
      value (cdr pair)
      old (getvar name))
    (if old (progn (setq changed (cons (cons name old)
            changed))
        (setvar name value))))
  (reverse changed))
```
</details>

---

### vectra:sqr

**用法**:
```lisp
(vectra:sqr f)
```

**参数**: 1 f  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:sqr (f /)
  (* f f))
```
</details>

---

### vectra:ss->enames

**用法**:
```lisp
(vectra:ss->enames ss)
```

**参数**: 1 ss  : 选择集;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:ss->enames (ss / en handles n)
  (if ss (progn (repeat (setq n (sslength ss))
        (setq en (ssname ss (setq n (1- n)))
          handles (cons en handles)))))
  handles)
```
</details>

---

### vectra:ss->handles

**用法**:
```lisp
(vectra:ss->handles ss)
```

**参数**: 1 ss  : 选择集;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:ss->handles (ss / e)
  (mapcar (function (lambda (e)
        (vectra:dxf e 5)))
    (vectra:ss->enames ss)))
```
</details>

---

### vectra:ss-highlight

**用法**:
```lisp
(vectra:ss-highlight ss)
```

**参数**: 1 ss  : 选择集;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:ss-highlight (ss / en n)
  (if $p-ss-highlighted (progn (vectra:ss-highlight-inner $p-ss-highlighted 4)
      (setq $p-ss-highlighted nil)))
  (if ss (progn (vectra:ss-highlight-inner ss 3)
      (setq $p-ss-highlighted ss))))
```
</details>

---

### vectra:ss-highlight-inner

**用法**:
```lisp
(vectra:ss-highlight-inner ss status)
```

**参数**: 1 ss  : 选择集;2 status  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:ss-highlight-inner (ss status / en n)
  (if ss (progn (repeat (setq n (sslength ss))
        (setq en (ssname ss (setq n (1- n))))
        (redraw en status)))))
```
</details>

---

### vectra:startundomark

**用法**:
```lisp
(vectra:startundomark )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:startundomark nil (if (not $p-undo-started)
    (progn (vla-startundomark (vla-get-activedocument (vlax-get-acad-object)))
      (setq $p-undo-started t))))
```
</details>

---

### vectra:string-connect

**用法**:
```lisp
(vectra:string-connect lst delim)
```

**参数**: 1 lst  : 列表;2 delim  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:string-connect (lst delim / e str)
  (setq str (car lst)
    lst (cdr lst))
  (while (setq e (car lst))
    (setq str (strcat str delim e))
    (setq lst (cdr lst)))
  str)
```
</details>

---

### vectra:string-left

**用法**:
```lisp
(vectra:string-left str n)
```

**参数**: 1 str  : 字符串;2 n  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:string-left (str n)
  (substr str 1 n))
```
</details>

---

### vectra:string-right

**用法**:
```lisp
(vectra:string-right str n)
```

**参数**: 1 str  : 字符串;2 n  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:string-right (str n / s sl)
  (setq sl (strlen str)
    s (- sl n -1))
  (if (< s 1)
    (setq s 1))
  (substr str s n))
```
</details>

---

### vectra:string-setnotempty

**用法**:
```lisp
(vectra:string-setnotempty symbol value)
```

**参数**: 1 symbol  : 未识别定义;2 value  : 值;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:string-setnotempty (symbol value /)
  (if (or (null (vl-symbol-value symbol))
      (equal (vl-symbol-value symbol)
        ""))
    (set symbol value)))
```
</details>

---

### vectra:string-subst

**用法**:
```lisp
(vectra:string-subst str find repl)
```

**参数**: 1 str  : 字符串;2 find  : 未识别定义;3 repl  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:string-subst (str find repl / pos len)
  (setq len (strlen repl)
    pos 0)
  (while (setq pos (vl-string-search find str pos))
    (setq str (vl-string-subst repl find str pos)
      pos (+ pos len)))
  str)
```
</details>

---

### vectra:string-substp

**用法**:
```lisp
(vectra:string-substp str find repl p)
```

**参数**: 1 str  : 字符串;2 find  : 未识别定义;3 repl  : 未识别定义;4 p  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:string-substp (str find repl p / pos len)
  (setq len (strlen find)
    pos 0 occ 0)
  (while (and (setq pos (vl-string-search find str pos))
      (< (setq occ (1+ occ))
        p))
    (setq pos (+ pos len)))
  (if (= occ p)
    (vl-string-subst repl find str pos)
    str))
```
</details>

---

### vectra:string-tokenize

**用法**:
```lisp
(vectra:string-tokenize str delim)
```

**参数**: 1 str  : 字符串;2 delim  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:string-tokenize (str delim / buff l2)
  (setq str (vl-string->list str)
    delim (vl-string->list delim))
  (while str (if (member (car str)
        delim)
      (setq l2 (cons (vl-list->string (reverse buff))
          l2)
        buff nil)
      (setq buff (cons (car str)
          buff)))
    (setq str (cdr str)))
  (setq l2 (cons (vl-list->string (reverse buff))
      l2))
  (reverse l2))
```
</details>

---

### vectra:stringp

**用法**:
```lisp
(vectra:stringp v)
```

**参数**: 1 v  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:stringp (v)
  (= (quote str)
    (type v)))
```
</details>

---

### vectra:tan

**用法**:
```lisp
(vectra:tan a)
```

**参数**: 1 a  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:tan (a)
  (/ (sin a)
    (cos a)))
```
</details>

---

### vectra:template-eval

**用法**:
```lisp
(vectra:template-eval template params)
```

**参数**: 1 template  : 未识别定义;2 params  : 参数列表;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:template-eval (template params / r)
  (apply (quote strcat)
    (mapcar (function (lambda (e)
          (if (vl-consp e)
            (if (setq r (vectra:get params (car e)))
              (cond ((numberp r)
                  (setq dig (cadr e))
                  (if (null dig)
                    (setq dig 0)
                    (setq dig (atoi dig)))
                  (vectra:number-padding-last r dig))
                (t (vl-princ-to-string r)))
              (strcat "{"
                (car e)
                "}"))
            e)))
      (vectra:template-parse template))))
```
</details>

---

### vectra:template-parse

**用法**:
```lisp
(vectra:template-parse template)
```

**参数**: 1 template  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:template-parse (template /)
  (vectra:template-parse-inner template "{"
    "}"))
```
</details>

---

### vectra:template-parse-inner

**用法**:
```lisp
(vectra:template-parse-inner template gs ge)
```

**参数**: 1 template  : 未识别定义;2 gs  : 未识别定义;3 ge  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:template-parse-inner (template gs ge / buff ch result)
  (setq template (vl-string->list template))
  (while (setq ch (car template))
    (cond ((= ch (ascii gs))
        (if buff (setq result (cons (vl-list->string (reverse buff))
              result)
            buff nil)))
      ((= ch (ascii ge))
        (if buff (setq result (cons (list (vl-list->string (reverse buff)))
              result)
            buff nil)))
      (t (setq buff (cons ch buff))))
    (setq template (cdr template)))
  (if buff (setq result (cons (vl-list->string (reverse buff))
        result)))
  (mapcar (function (lambda (e)
        (if (atom e)
          e (vectra:string-tokenize (car e)
            ":"))))
    (reverse result)))
```
</details>

---

### vectra:textstyle-get

**用法**:
```lisp
(vectra:textstyle-get name font bigfont width)
```

**参数**: 1 name  : 未识别定义;2 font  : 未识别定义;3 bigfont  : 未识别定义;4 width  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:textstyle-get (name font bigfont width / style styles)
  (setq styles (vla-get-textstyles (vla-get-activedocument (vlax-get-acad-object))))
  (if (vl-catch-all-error-p (setq style (vl-catch-all-apply (quote vla-item)
          (list styles name))))
    (progn (setq style (vla-add styles name))
      (vla-put-fontfile style font)
      (vla-put-bigfontfile style bigfont)
      (vla-put-width style width)))
  style)
```
</details>

---

### vectra:timer-start

**用法**:
```lisp
(vectra:timer-start )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:timer-start nil (setq $p-timestart (vectra:timestamp)))
```
</details>

---

### vectra:timer-stop

**用法**:
```lisp
(vectra:timer-stop )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:timer-stop (/ el)
  (setq el (- (vectra:timestamp)
      $p-timestart))
  (setq $p-timestart (vectra:timestamp))
  el)
```
</details>

---

### vectra:timestamp

**用法**:
```lisp
(vectra:timestamp )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:timestamp nil (getvar "MILLISECS"))
```
</details>

---

### vectra:trp

**用法**:
```lisp
(vectra:trp m)
```

**参数**: 1 m  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:trp (m)
  (apply (quote mapcar)
    (cons (quote list)
      m)))
```
</details>

---

### vectra:uid

**用法**:
```lisp
(vectra:uid )
```

**参数**: None

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:uid (/)
  (if (null $p-uid-base)
    (setq $p-uid-base 1))
  (setq $p-uid-base (abs (vectra:hash-1 (getvar (quote millisecs))
        $p-uid-base))))
```
</details>

---

### vectra:unset

**用法**:
```lisp
(vectra:unset lst keys)
```

**参数**: 1 lst  : 列表;2 keys  : 键，关键字;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:unset (lst keys /)
  (if (atom keys)
    (setq keys (list keys)))
  (vl-remove-if (function (lambda (e)
        (member (car e)
          keys)))
    lst))
```
</details>

---

### vectra:var->list

**用法**:
```lisp
(vectra:var->list var)
```

**参数**: 1 var  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:var->list (var / e)
  (cond ((= (quote safearray)
        (type var))
      (mapcar (quote (lambda (e)
            (vectra:var->list e)))
        (vlax-safearray->list var)))
    ((= (quote variant)
        (type var))
      (vectra:var->list (vlax-variant-value var)))
    (t var)))
```
</details>

---

### vectra:vector-angle

**用法**:
```lisp
(vectra:vector-angle v)
```

**参数**: 1 v  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:vector-angle (v /)
  (atan (cadr v)
    (car v)))
```
</details>

---

### vectra:vector-angle2

**用法**:
```lisp
(vectra:vector-angle2 v1 v2)
```

**参数**: 1 v1  : 未识别定义;2 v2  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:vector-angle2 (v1 v2 / rt)
  (vectra:acos (vectra:vector-dotproduct (vectra:vector-normal v1)
      (vectra:vector-normal v2))))
```
</details>

---

### vectra:vector-dotproduct

**用法**:
```lisp
(vectra:vector-dotproduct v1 v2)
```

**参数**: 1 v1  : 未识别定义;2 v2  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:vector-dotproduct (v1 v2 /)
  (apply (quote +)
    (mapcar (quote *)
      v1 v2)))
```
</details>

---

### vectra:vector-from2p

**用法**:
```lisp
(vectra:vector-from2p p1 p2)
```

**参数**: 1 p1  : 未识别定义;2 p2  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:vector-from2p (p1 p2 /)
  (vectra:vector-normal (mapcar (quote -)
      p2 p1)))
```
</details>

---

### vectra:vector-len

**用法**:
```lisp
(vectra:vector-len v)
```

**参数**: 1 v  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:vector-len (v /)
  (sqrt (apply (quote +)
      (mapcar (quote *)
        v v))))
```
</details>

---

### vectra:vector-normal

**用法**:
```lisp
(vectra:vector-normal v)
```

**参数**: 1 v  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:vector-normal (v / len)
  (setq len (vectra:vector-len v))
  (if (equal len 0.0 1.0e-06)
    v (mapcar (function (lambda (e)
          (/ e len)))
      v)))
```
</details>

---

### vectra:vector-reverse

**用法**:
```lisp
(vectra:vector-reverse v)
```

**参数**: 1 v  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:vector-reverse (v /)
  (mapcar (quote -)
    v))
```
</details>

---

### vectra:xdata-all

**用法**:
```lisp
(vectra:xdata-all ename)
```

**参数**: 1 ename  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xdata-all (ename /)
  (vectra:xdata-get-inner ename "*"))
```
</details>

---

### vectra:xdata-exist

**用法**:
```lisp
(vectra:xdata-exist ename key)
```

**参数**: 1 ename  : 单个图元;2 key  : 键，关键字;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xdata-exist (ename key /)
  (member key (vectra:xdata-keys ename)))
```
</details>

---

### vectra:xdata-get

**用法**:
```lisp
(vectra:xdata-get ename appname)
```

**参数**: 1 ename  : 单个图元;2 appname  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xdata-get (ename appname /)
  (cdar (vectra:xdata-get-inner ename appname)))
```
</details>

---

### vectra:xdata-get-inner

**用法**:
```lisp
(vectra:xdata-get-inner ename appname)
```

**参数**: 1 ename  : 单个图元;2 appname  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xdata-get-inner (ename appname /)
  (cdr (assoc -3 (entget ename (list appname)))))
```
</details>

---

### vectra:xdata-keys

**用法**:
```lisp
(vectra:xdata-keys ename)
```

**参数**: 1 ename  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xdata-keys (ename /)
  (mapcar (quote car)
    (vectra:xdata-all ename)))
```
</details>

---

### vectra:xdata-remove

**用法**:
```lisp
(vectra:xdata-remove ename appid)
```

**参数**: 1 ename  : 单个图元;2 appid  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xdata-remove (ename appid /)
  (cond ((= "*"
        appid)
      (vectra:xdata-set-inner ename (mapcar (quote list)
          (vectra:xdata-keys ename))))
    ((listp appid)
      (vectra:xdata-set-inner ename (mapcar (quote list)
          appid)))
    (t (vectra:xdata-set-inner ename (list (cons appid nil))))))
```
</details>

---

### vectra:xdata-set

**用法**:
```lisp
(vectra:xdata-set ename appid xdata)
```

**参数**: 1 ename  : 单个图元;2 appid  : 未识别定义;3 xdata  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xdata-set (ename appid xdata /)
  (if (null (tblsearch "APPID"
        appid))
    (regapp appid))
  (vectra:xdata-set-inner ename (list (cons appid xdata))))
```
</details>

---

### vectra:xdata-set-inner

**用法**:
```lisp
(vectra:xdata-set-inner ename xdata)
```

**参数**: 1 ename  : 单个图元;2 xdata  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xdata-set-inner (ename xdata /)
  (entmod (list (cons -1 ename)
      (cons -3 xdata))))
```
</details>

---

### vectra:xprop-exist

**用法**:
```lisp
(vectra:xprop-exist ename appname names)
```

**参数**: 1 ename  : 单个图元;2 appname  : 未识别定义;3 names  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xprop-exist (ename appname names / rv)
  (setq rv (vectra:xprop-get ename appname names))
  (cond ((atom names)
      rv)
    ((vl-consp names)
      (= (length names)
        (length rv)))))
```
</details>

---

### vectra:xprop-get

**用法**:
```lisp
(vectra:xprop-get ename appname names)
```

**参数**: 1 ename  : 单个图元;2 appname  : 未识别定义;3 names  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xprop-get (ename appname names / xdata rv)
  (setq xdata (vectra:xdata-get ename appname))
  (cond ((atom names)
      (while xdata (if (= names (cdar xdata))
          (setq rv (cdadr xdata)
            xdata nil))
        (setq xdata (cddr xdata))))
    ((vl-consp names)
      (while xdata (if (member (cdar xdata)
            names)
          (setq rv (cons (cons (cdar xdata)
                (cdadr xdata))
              rv)
            names (vl-remove (cdar xdata)
              names)))
        (setq xdata (cddr xdata)))))
  rv)
```
</details>

---

### vectra:xprop-getall

**用法**:
```lisp
(vectra:xprop-getall ename appname)
```

**参数**: 1 ename  : 单个图元;2 appname  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xprop-getall (ename appname /)
  (vectra:xprop-unpack (vectra:xdata-get ename appname)))
```
</details>

---

### vectra:xprop-pack

**用法**:
```lisp
(vectra:xprop-pack properties xdatatypes)
```

**参数**: 1 properties  : 未识别定义;2 xdatatypes  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xprop-pack (properties xdatatypes / enc rv)
  (foreach e properties (setq enc (vectra:xprop-pack1 e xdatatypes))
    (setq rv (cons (car enc)
        rv)
      rv (cons (cadr enc)
        rv)))
  (reverse rv))
```
</details>

---

### vectra:xprop-pack1

**用法**:
```lisp
(vectra:xprop-pack1 kv xdatatypes)
```

**参数**: 1 kv  : 未识别定义;2 xdatatypes  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xprop-pack1 (kv xdatatypes / code name value)
  (setq name (car kv)
    value (cdr kv))
  (or (setq code (cdr (assoc name xdatatypes)))
    (setq code (cdr (assoc (type value)
          (quote ((str . 1000)
              (int . 1070)
              (real . 1040)
              (list . 1010)))))))
  (list (cons 1000 name)
    (cons code value)))
```
</details>

---

### vectra:xprop-remove

**用法**:
```lisp
(vectra:xprop-remove ename appname names)
```

**参数**: 1 ename  : 单个图元;2 appname  : 未识别定义;3 names  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xprop-remove (ename appname names / xdata xdata2)
  (if (and names (atom names))
    (setq names (list names)))
  (if names (progn (setq xdata (vectra:xdata-get ename appname))
      (while xdata (if (member (cdar xdata)
            names)
          (setq names (vl-remove (cdar xdata)
              names))
          (setq xdata2 (cons (car xdata)
              xdata2)
            xdata2 (cons (cadr xdata)
              xdata2)))
        (setq xdata (cddr xdata)))
      (setq xdata2 (reverse xdata2))
      (vectra:xdata-set ename appname xdata2))))
```
</details>

---

### vectra:xprop-replace

**用法**:
```lisp
(vectra:xprop-replace ename appname prop datadef)
```

**参数**: 1 ename  : 单个图元;2 appname  : 未识别定义;3 prop  : 未识别定义;4 datadef  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xprop-replace (ename appname prop datadef / xdata xdata2 tmp)
  (if (atom (car prop))
    (setq prop (list prop)))
  (vectra:xdata-set ename appname (vectra:xprop-pack prop datadef)))
```
</details>

---

### vectra:xprop-set

**用法**:
```lisp
(vectra:xprop-set ename appname prop)
```

**参数**: 1 ename  : 单个图元;2 appname  : 未识别定义;3 prop  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xprop-set (ename appname prop /)
  (vectra:xprop-set-inner ename appname prop nil))
```
</details>

---

### vectra:xprop-set-inner

**用法**:
```lisp
(vectra:xprop-set-inner ename appname prop datadef)
```

**参数**: 1 ename  : 单个图元;2 appname  : 未识别定义;3 prop  : 未识别定义;4 datadef  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xprop-set-inner (ename appname prop datadef / xdata xdata2 tmp)
  (if (atom (car prop))
    (setq prop (list prop)))
  (setq prop (vl-remove-if (quote (lambda (e)
          (or (null (car e))
            (= ""
              (car e))
            (null (cdr e))
            (vl-catch-all-error-p (cdr e)))))
      prop))
  (if prop (progn (setq xdata (vectra:xdata-get ename appname))
      (while xdata (if (setq tmp (assoc (cdar xdata)
              prop))
          (setq xdata2 (cons (car xdata)
              xdata2)
            xdata2 (cons (cons (caadr xdata)
                (cdr tmp))
              xdata2)
            prop (vl-remove tmp prop))
          (setq xdata2 (cons (car xdata)
              xdata2)
            xdata2 (cons (cadr xdata)
              xdata2)))
        (setq xdata (cddr xdata)))
      (if prop (foreach e prop (setq xdata2 (append (reverse (vectra:xprop-pack1 e datadef))
              xdata2))))
      (setq xdata2 (reverse xdata2))
      (vectra:xdata-set ename appname xdata2))))
```
</details>

---

### vectra:xprop-unpack

**用法**:
```lisp
(vectra:xprop-unpack xdata)
```

**参数**: 1 xdata  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vectra:xprop-unpack (xdata / rv)
  (while xdata (setq rv (cons (cons (cdar xdata)
          (cdadr xdata))
        rv)
      xdata (cddr xdata)))
  (reverse rv))
```
</details>

---

## vitalgg

**模块说明**: 专用功能模块

### vitalgg:helloworld

**说明**: 函数功能说明，以及参数说明，作者等信息

**用法**:
```lisp
(vitalgg:helloworld )
```

**参数**: None

**返回值**: 返回值类型及说明

**示例**:
```lisp
示例
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vitalgg:helloworld nil "函数功能说明，以及参数说明，作者等信息"
  "返回值类型及说明"
  "示例"
  (alert "Hello autolisp!")
  "注释文字会被清除掉。必要的文字，请直接写字符串，就像该行这样。")
```
</details>

---

### vitalgg:test

**说明**: 用于测试的函数，str 为字符串，by VitalGG

**用法**:
```lisp
(vitalgg:test str)
```

**参数**: 1 str  : 字符串;

**返回值**: int: 参数字符串的第一个字母的 ascii 码

**示例**:
```lisp
(vitalgg:test "Match")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vitalgg:test (str)
  "用于测试的函数，str 为字符串，by VitalGG"
  "int: 参数字符串的第一个字母的 ascii 码"
  "(vitalgg:test \"Match\")"
  "生成字符串的第一个ascii码。"
  (ascii str))
```
</details>

---

## vla

**模块说明**: 专用功能模块

### vla:buildfilter

**说明**: 构建variant列表;  参数：;  filter:点对列表

**用法**:
```lisp
(vla:buildfilter filter)
```

**参数**: 1 filter  : 过滤dxf组码;

**返回值**: variant列表

**示例**:
```lisp
(vla:buildFilter '((1 . "123")(2 . "4556")))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vla:buildfilter (filter)
    "构建variant列表\n参数：\nfilter:点对列表"
    "variant列表"
    "(vla:buildfilter '((1 . \"123\")(2 . \"4556\")))"
    (vl-load-com)
    (mapcar (quote (lambda (lst typ)
                (vlax-make-variant (vlax-safearray-fill (vlax-make-safearray typ (cons 0 (1- (length lst))))
                        lst))))
        (list (mapcar (quote car)
                filter)
            (mapcar (quote cdr)
                filter))
        (list vlax-vbinteger vlax-vbvariant)))
```
</details>

---

### vla:dump

**说明**: 列对象属性和方法。

**用法**:
```lisp
(vla:dump obj)
```

**参数**: 1 obj  : activeX 对象;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vla:dump (obj)
  "列对象属性和方法。"
  (vlax-dump-object obj t)
  (setq atoms (atoms-family 1))
  (setq allproperties (mapcar (quote (lambda (x)
          (substr x 9)))
      (vl-remove-if-not (quote (lambda (x)
            (wcmatch x "VLA-GET-*")))
        atoms)))
  (setq allmethods (mapcar (quote (lambda (x)
          (substr x 5)))
      (vl-remove-if-not (quote (lambda (x)
            (and (wcmatch x "VLA-*")
              (wcmatch x "~VLA-GET-*")
              (wcmatch x "~VLA-PUT-*"))))
        atoms)))
  (setq properties (vl-remove-if-not (quote (lambda (x)
          (vlax-property-available-p obj x)))
      allproperties))
  (setq methods (vl-remove-if-not (quote (lambda (x)
          (vlax-method-applicable-p obj x)))
      allmethods))
  (list (cadr (string:to-list (vl-princ-to-string obj)
        "
        "))
    properties methods))
```
</details>

---

### vla:enamelist->vla

**说明**: 图元列表转为Vla列表。lst:图元列表

**用法**:
```lisp
(vla:enamelist->vla lst)
```

**参数**: 1 lst  : 列表;

**返回值**: Vla列表

**示例**:
```lisp
(vla:enamelist->vla lst)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vla:enamelist->vla (lst)
  "图元列表转为Vla列表。lst:图元列表"
  "Vla列表"
  "(vla:enamelist->vla lst)"
  (mapcar (quote vlax-ename->vla-object)
    lst))
```
</details>

---

### vla:get-property

**说明**: 点操作符方式获取对象的属性，参数:sym vla对象的符号，str 用点表示的CAD对象层级的属性名。str的中首个类名需与sym对象的类一致。

**用法**:
```lisp
(vla:get-property sym str)
```

**参数**: 1 sym  : 未识别定义;2 str  : 字符串;

**返回值**: any

**示例**:
```lisp
(vla:get-property *ACAD* "application.preferences.drafting.autosnapmarkersize")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vla:get-property (sym str / obj-tree res)
  "点操作符方式获取对象的属性，参数:sym vla对象的符号，str 用点表示的CAD对象层级的属性名。str的中首个类名需与sym对象的类一致。"
  "any"
  "(vla:get-property *ACAD* \"application.preferences.drafting.autosnapmarkersize\")"
  (if (null acadobj)
    (setq acadobj (vlax-get-acad-object)))
  (setq obj-tree (string:to-list str "."))
  (setq res sym)
  (while (and (p:vlap res)
      (cdr obj-tree))
    (setq porm (string:to-list (cadr obj-tree)
        "("))
      (cond ((vlax-property-available-p res (read (car porm)))
          (setq res (vlax-get-property res (read (car (setq obj-tree (cdr obj-tree)))))))
        ((vlax-method-applicable-p res (read (car porm)))
          (setq params (mapcar (quote read)
              (string:to-list (vl-string-right-trim ")"
                (cadr porm))
              ",")))
        (setq res (apply (quote vlax-invoke)
            (cons res (cons (read (car porm))
                params))))
        (setq obj-tree (cdr obj-tree)))))
  res)
```
</details>

---

### vla:get-value

**说明**: 变体里取值.参数 var:变体或者数组

**用法**:
```lisp
(vla:get-value var)
```

**参数**: 1 var  : 未明确定义;

**返回值**: 数据列表

**示例**:
```lisp
(vla:get-value var)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vla:get-value (var)
  "变体里取值.参数 var:变体或者数组"
  "数据列表"
  "(vla:get-value var)"
  (cond ((listp var)
      (mapcar (quote vla:get-value)
        var))
    ((p:variantp var)
      (vla:get-value (vlax-variant-value var)))
    ((p:safearrayp var)
      (mapcar (quote vla:get-value)
        (vlax-safearray->list var)))
    (t var)))
```
</details>

---

### vla:list->array

**说明**: 表->安全数组类型（一维数组）;  参数：;  nlist:列表，要求数据的类型要和arraytype一致;  arraytype:可指定如下常量：可以用后面的数字也可以用前面的类型符号;

**用法**:
```lisp
(vla:list->array nlist arraytype)
```

**参数**: 1 nlist  : 未明确定义;  2 arraytype  : 未明确定义;

**返回值**: 一维数组

**示例**:
```lisp
(vla:List->Array '(1 2 3 4) vlax-vbInteger)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vla:list->array (nlist arraytype)
    "表->安全数组类型（一维数组）\n参数：\nnlist:列表，要求数据的类型要和arraytype一致\narraytype:可指定如下常量：可以用后面的数字也可以用前面的类型符号\n"
    "一维数组"
    "(vla:list->array '(1 2 3 4)
        vlax-vbinteger)"
    (vlax-safearray-fill (vlax-make-safearray arraytype (cons 0 (1- (length nlist))))
        nlist))
```
</details>

---

### vla:list->arrays

**说明**: 表->安全数组类型（多维数组，多于二维报错）;  arg:;  nlist:列表，要求数据的类型要和arraytype一致，表的各维必须为表;  arraytype:可指定如下常量：可以用后面的数字也可以用前面的类型符号

**用法**:
```lisp
(vla:list->arrays nlist arraytype)
```

**参数**: 1 nlist  : 未明确定义;  2 arraytype  : 未明确定义;

**返回值**: 多维数组

**示例**:
```lisp
vla:List->Arrays '((1 2) (3 4)) vlax-vbInteger)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vla:list->arrays (nlist arraytype)
    "表->安全数组类型（多维数组，多于二维报错）\narg:\nnlist:列表，要求数据的类型要和arraytype一致，表的各维必须为表\narraytype:可指定如下常量：可以用后面的数字也可以用前面的类型符号"
    "多维数组"
    "vla:list->arrays '((1 2)
        (3 4))
    vlax-vbinteger)"
(vlax-safearray-fill (apply (quote vlax-make-safearray)
        (cons arraytype (mapcar (quote (lambda (x)
                        (cons 0 (1- (length x)))))
                nlist)))
    nlist))
```
</details>

---

### vla:objarray

**说明**: 创建vla对象数组;  参    数:lst:vla对象表

**用法**:
```lisp
(vla:objarray lst)
```

**参数**: 1 lst  : 列表;

**返回值**: 返 回 值:vla对象数组

**示例**:
```lisp
(vla:ObjArray lst)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vla:objarray (lst)
    "创建vla对象数组\n参    数:lst:vla对象表"
    "返 回 值:vla对象数组"
    "(vla:objarray lst)"
    (vla:list->array lst vlax-vbobject))
```
</details>

---

### vla:objectvariant

**说明**: 创建vla对象表变体.;  参数：;  lst:vla对象表

**用法**:
```lisp
(vla:objectvariant lst)
```

**参数**: 1 lst  : 列表;

**返回值**: 变体

**示例**:
```lisp
(vla:ObjectVariant lst)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vla:objectvariant (lst)
    "创建vla对象表变体.\n参数：\nlst:vla对象表"
    "变体"
    "(vla:objectvariant lst)"
    (vlax-make-variant (vlax-safearray-fill (vlax-make-safearray vlax-vbobject (cons 0 (1- (length lst))))
            lst)))
```
</details>

---

### vla:prototype

**说明**: 返回对象类、属性和方法组成的列表。

**用法**:
```lisp
(vla:prototype obj)
```

**参数**: 1 obj  : activeX 对象;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vla:prototype (obj)
  "返回对象类、属性和方法组成的列表。"
  (setq atoms (atoms-family 1))
  (setq allproperties (mapcar (quote (lambda (x)
          (substr x 9)))
      (vl-remove-if-not (quote (lambda (x)
            (wcmatch x "VLA-GET-*")))
        atoms)))
  (setq allmethods (mapcar (quote (lambda (x)
          (substr x 5)))
      (vl-remove-if-not (quote (lambda (x)
            (and (wcmatch x "VLA-*")
              (wcmatch x "~VLA-GET-*")
              (wcmatch x "~VLA-PUT-*"))))
        atoms)))
  (setq properties (vl-remove-if-not (quote (lambda (x)
          (vlax-property-available-p obj x)))
      allproperties))
  (setq methods (vl-remove-if-not (quote (lambda (x)
          (vlax-method-applicable-p obj x)))
      allmethods))
  (list (substr (cadr (string:to-list (vl-princ-to-string obj)
          "
          "))
      6)
    properties methods))
```
</details>

---

### vla:put-property

**说明**: 点操作符方式设置对象的属性，参数:sym vla对象的符号，str 用点表示的CAD对象层级的属性名。

**用法**:
```lisp
(vla:put-property sym str value)
```

**参数**: 1 sym  : 未识别定义;2 str  : 字符串;3 value  : 值;

**返回值**: any

**示例**:
```lisp
(vla:put-property *ACAD* "application.preferences.drafting.autosnapmarkersize" 5)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vla:put-property (sym str value / obj-tree res)
  "点操作符方式设置对象的属性，参数:sym vla对象的符号，str 用点表示的CAD对象层级的属性名。"
  "any"
  "(vla:put-property *ACAD* \"application.preferences.drafting.autosnapmarkersize\"
    5)"
  (if (null acadobj)
    (setq acadobj (vlax-get-acad-object)))
  (setq obj-tree (string:to-list str "."))
  (setq res sym)
  (while (and (p:vlap res)
      (cddr obj-tree))
    (setq porm (string:to-list (cadr obj-tree)
        "("))
      (cond ((vlax-property-available-p res (read (car porm)))
          (setq res (vlax-get-property res (read (car (setq obj-tree (cdr obj-tree)))))))
        ((vlax-method-applicable-p res (read (car porm)))
          (setq params (mapcar (quote read)
              (string:to-list (vl-string-right-trim ")"
                (cadr porm))
              ",")))
        (setq res (apply (quote vlax-invoke)
            (cons res (cons (read (car porm))
                params))))
        (setq obj-tree (cdr obj-tree)))))
  (vlax-put-property res (read (car (setq obj-tree (cdr obj-tree))))
    value))
```
</details>

---

### vla:sel

**说明**: 单选对象。

**用法**:
```lisp
(vla:sel )
```

**参数**: None

**返回值**: VLA-OBJECT 对象

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vla:sel nil "单选对象。"
    "vla-object 对象"
    (e2o (car (entsel))))
```
</details>

---

### vla:to-ename

**说明**: object转eName,简化函数 o2e.

**用法**:
```lisp
(vla:to-ename obj)
```

**参数**: 1 obj  : activeX 对象;

**返回值**: ename entity

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun vla:to-ename (obj)
    "object转ename,简化函数 o2e."
    "ename entity"
    (vlax-vla-object->ename obj))
```
</details>

---

## word

**模块说明**: 专用功能模块

### word:close

**说明**: 关闭 word 文档

**用法**:
```lisp
(word:close ax-doc)
```

**参数**: 1 ax-doc  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun word:close (ax-doc)
  "关闭 word 文档"
  (vlax-invoke ax-doc (quote close)))
```
</details>

---

### word:example

**说明**: word 类函数调用示例

**用法**:
```lisp
(word:example filename str)
```

**参数**: 1 filename  : 文件名;2 str  : 字符串;

**返回值**: nil

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun word:example (filename str / ax-word ax-doc)
  "word 类函数调用示例"
  "nil"
  ""
  "建立 word app"
  (setq ax-word (word:open filename nil))
  "文档对象 ax-doc"
  (setq ax-doc (vlax-get-property ax-word (quote activedocument)))
  "向文档对象写内容"
  (word:write-line ax-doc str)
  "关闭文档"
  (word:close ax-doc)
  "关闭 app"
  (word:quit ax-word t))
```
</details>

---

### word:new

**说明**: 新建word 文档参数:ishide:是否可见，t为可见，nil为不可见

**用法**:
```lisp
(word:new ishide)
```

**参数**: 1 ishide  : 未识别定义;

**返回值**: 一个表示word的vla对象

**示例**:
```lisp
(word:New t)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun word:new (ishide / ax-word)
  "新建word 文档\n参数:ishide:是否可见，t为可见，nil为不可见"
  "一个表示word的vla对象"
  "(word:New t)"
  (if (setq ax-word (vlax-get-or-create-object "Word.Application"))
    (progn (vlax-invoke (vlax-get-property ax-word (quote workbooks))
        (quote add))
      (if ishide (vla-put-visible ax-word 1)
        (vla-put-visible ax-word 0))))
  ax-word)
```
</details>

---

### word:open

**说明**: 打开一个word文件参数:Filename:文件路径参数:ishide:是否可见，t为可见，nil为不可见

**用法**:
```lisp
(word:open filename ishide)
```

**参数**: 1 filename  : 文件名;2 ishide  : 未识别定义;

**返回值**: 一个表示打开的 word 文件的vla对象

**示例**:
```lisp
(word:open "D:\1.docx" t)
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun word:open (filename ishide / ax-word ax-doc)
  "打开一个word文件\n参数:Filename:文件路径\n参数:ishide:是否可见，t为可见，nil为不可见"
  "一个表示打开的 word 文件的vla对象"
  "(word:open \"D:\\1.docx\"
    t)"
  (if (setq ax-word (vlax-get-or-create-object "Word.Application"))
    (progn (if ishide (vla-put-visible ax-word :vlax-true)
        (vla-put-visible ax-word :vlax-false))
      (if (findfile filename)
        (vlax-invoke (vlax-get-property ax-word (quote documents))
          (quote open)
          filename)
        (progn (vlax-invoke (vlax-get-property ax-word (quote documents))
            (quote add))
          (vlax-invoke (vlax-get-property ax-word (quote activedocument))
            (quote saveas)
            filename)))))
  ax-word)
```
</details>

---

### word:quit

**说明**: 退出Word参数:ax-word:打开的word对象参数:SaveYN:是否保存，t为保存，nil为不保存

**用法**:
```lisp
(word:quit ax-word saveyn)
```

**参数**: 1 ax-word  : 未识别定义;2 saveyn  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun word:quit (ax-word saveyn)
  "退出Word\n参数:ax-word:打开的word对象\n参数:SaveYN:是否保存，t为保存，nil为不保存"
  (if (> (vlax-get-property (vlax-get-property ax-word (quote documents))
        (quote count))
      0)
    (if saveyn (vlax-invoke (vlax-get-property ax-word "ActiveDocument")
        (quote close))
      (vlax-invoke (vlax-get-property ax-word "ActiveDocument")
        (quote close)
        :vlax-false)))
  (vlax-invoke ax-word (quote quit))
  (vlax-release-object ax-word)
  (setq ax-word nil)
  (gc))
```
</details>

---

### word:save

**说明**: 保存 word 文档，如果是新建的文档，需要先指定文件路径。

**用法**:
```lisp
(word:save ax-doc)
```

**参数**: 1 ax-doc  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun word:save (ax-doc)
  "保存 word 文档，如果是新建的文档，需要先指定文件路径。"
  (vlax-invoke ax-doc (quote save)))
```
</details>

---

### word:write-line

**说明**: 向 word 的文档对象最后写入文本。

**用法**:
```lisp
(word:write-line ax-doc text)
```

**参数**: 1 ax-doc  : 未识别定义;2 text  : 未识别定义;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun word:write-line (ax-doc text / end)
  "向 word 的文档对象最后写入文本。"
  (setq end (vlax-get-property (vlax-get-property (vlax-get-property ax-doc (quote paragraphs))
        (quote last))
      (quote range)))
  (vlax-invoke end (quote insertafter)
    text))
```
</details>

---

## xdata

**模块说明**: 专用功能模块

### xdata:delete

**说明**: 删除图元的扩展数据

**用法**:
```lisp
(xdata:delete ename)
```

**参数**: 1 ename  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun xdata:delete (ename / lst)
  "删除图元的扩展数据"
  (cond ((p:enamep ename)
      (entmod (list (cons -1 ename)
          (cons -3 (mapcar (quote list)
              (mapcar (quote car)
                (cdr (assoc -3 (entget ename (quote ("*")))))))))))
    ((p:listp ename)
      (mapcar (quote xdata:delete)
        ename))
    ((p:picksetp ename)
      (mapcar (quote xdata:delete)
        (pickset:to-list ename)))))
```
</details>

---

### xdata:put

**说明**: 向图元 ename 附加扩展数据 values,values 为一个值或一些值的列表

**用法**:
```lisp
(xdata:put ename appid values)
```

**参数**: 1 ename  : 单个图元;2 appid  : 未识别定义;3 values  : 值;

**返回值**: ename

**示例**:
```lisp
(xdata:put(car(entsel)) "atlisp"  '(10 100))
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun xdata:put (ename appid values / xdata xdata-new)
  "向图元 ename 附加扩展数据 values,values 为一个值或一些值的列表"
  "ename"
  "(xdata:put(car(entsel))
    \"atlisp\"
     '(10 100))"
  (setq appid (strcase appid))
  (regapp appid)
  (if (atom values)
    (setq values (list values)))
  (setq xdata-new (vl-remove nil (mapcar (quote (lambda (x)
            (cond ((p:stringp x)
                (cons 1000 x))
              ((and (p:intp x)
                  (< (abs x)
                    32767))
                (cons 1070 x))
              ((and (p:intp x)
                  (> (abs x)
                    32767))
                (cons 1071 x))
              ((p:realp x)
                (cons 1040 x)))))
        values)))
  (setq xdata (cdr (assoc -3 (entget ename (quote ("*"))))))
  (if (assoc appid xdata)
    (setq xdata (subst (cons appid (append (cdr (assoc appid xdata))
            xdata-new))
        (assoc appid xdata)
        xdata))
    (setq xdata (append xdata (list (cons appid xdata-new)))))
  (entmod (append (entget ename)
      (list (cons -3 xdata)))))
```
</details>

---

### xdata:remove-all

**说明**: 删除图元的扩展数据

**用法**:
```lisp
(xdata:remove-all ename)
```

**参数**: 1 ename  : 单个图元;

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun xdata:remove-all (ename / lst)
  "删除图元的扩展数据"
  (cond ((p:enamep ename)
      (entmod (list (cons -1 ename)
          (cons -3 (mapcar (quote list)
              (mapcar (quote car)
                (cdr (assoc -3 (entget ename (quote ("*")))))))))))
    ((p:listp ename)
      (mapcar (quote xdata:remove-all)
        ename))
    ((p:picksetp ename)
      (mapcar (quote xdata:remove-all)
        (pickset:to-list ename)))))
```
</details>

---

### xdata:remove-appid

**说明**: 删除图元的应用名为appid的扩展数据

**用法**:
```lisp
(xdata:remove-appid ename appid)
```

**参数**: 1 ename  : 单个图元;2 appid  : 未识别定义;

**示例**:
```lisp
(xdata:remove-appid (car(entsel)) "ACAD")
```

<details>
<summary><strong>📋 查看源码</strong></summary>

```lisp
(defun xdata:remove-appid (ename appid / lst)
  "删除图元的应用名为appid的扩展数据"
  ""
  "(xdata:remove-appid (car(entsel))
    \"ACAD\")"
  (cond ((p:enamep ename)
      (entmod (list (cons -1 ename)
          (cons -3 (mapcar (quote (lambda (x)
                  (if (string-equal appid (car x))
                    (cons (car x)
                      nil)
                    x)))
              (cdr (assoc -3 (entget ename (quote ("*"))))))))))
    ((p:ename-listp ename)
      (mapcar (quote (lambda (x)
            (xdata:remove-appid x appid)))
        ename))
    ((p:picksetp ename)
      (mapcar (quote (lambda (x)
            (xdata:remove-appid x appid)))
        (pickset:to-list ename)))))
```
</details>

---

