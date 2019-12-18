# VRMC_vrm

## 目次

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Contributors](#contributors)
- [Status](#status)
- [Dependencies](#dependencies)
- [Overview](#overview)
  - [形式と拡張子](#%E5%BD%A2%E5%BC%8F%E3%81%A8%E6%8B%A1%E5%BC%B5%E5%AD%90)
  - [バージョン](#%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3)
  - [モデルのメタ情報](#%E3%83%A2%E3%83%87%E3%83%AB%E3%81%AE%E3%83%A1%E3%82%BF%E6%83%85%E5%A0%B1)
  - [ヒューマノイド](#%E3%83%92%E3%83%A5%E3%83%BC%E3%83%9E%E3%83%8E%E3%82%A4%E3%83%89)
  - [モデルの正規化](#%E3%83%A2%E3%83%87%E3%83%AB%E3%81%AE%E6%AD%A3%E8%A6%8F%E5%8C%96)
  - [BlendShape](#blendshape)
  - [一人称](#%E4%B8%80%E4%BA%BA%E7%A7%B0)
  - [視線制御](#%E8%A6%96%E7%B7%9A%E5%88%B6%E5%BE%A1)
  - [SpringBone](#springbone)
  - [Material](#material)
  - [Constraint](#constraint)
  - [更新の適用順](#%E6%9B%B4%E6%96%B0%E3%81%AE%E9%81%A9%E7%94%A8%E9%A0%86)
- [glTF Schema Updates](#gltf-schema-updates)
  - [座標の単位](#%E5%BA%A7%E6%A8%99%E3%81%AE%E5%8D%98%E4%BD%8D)
  - [使わない項目](#%E4%BD%BF%E3%82%8F%E3%81%AA%E3%81%84%E9%A0%85%E7%9B%AE)
  - [各種名前の制約](#%E5%90%84%E7%A8%AE%E5%90%8D%E5%89%8D%E3%81%AE%E5%88%B6%E7%B4%84)
  - [Meshの格納の制約](#mesh%E3%81%AE%E6%A0%BC%E7%B4%8D%E3%81%AE%E5%88%B6%E7%B4%84)
- [JSON Schema](#json-schema)
- [Known Implementations](#known-implementations)
  - [VRM-0.0](#vrm-00)
- [Resources](#resources)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Contributors

* 進藤哲郎
* 廣瀬 淳一
* 蘇柏彰

## Status

* In development

## Dependencies

Written against the glTF 2.0 spec.

* require VRMC_materials_mtoon extension
* require KHR_materials_unlit extension
* require KHR_texture_transform extension

## Overview

GLTFシーンにVRアバター向けの人間型モデル一体分の情報を格納します。
VRM拡張に追加情報を保持するとともに既存のGLTF部分にも追加の制約を課すことで、人型モデルをプログラムから統一的に操作できるようにします。

### 形式と拡張子

`.glb` 形式で保存し拡張子に `.vrm` を使います。

### バージョン

`extensions.VRMC_vrm`

| 名前        | 備考                |
|:------------|:--------------------|
| specVersion | VRMの仕様バージョン |

エクスポーター実装のバージョンは、 `assets.generator` に出力します。

### モデルのメタ情報

`extensions.VRMC_vrm.meta`

#### モデル名・作者など

| 名前               | 備考                                                                                                        |
|:-------------------|:------------------------------------------------------------------------------------------------------------|
| title              | アバターモデルの名前を設定します                                                                            |
| version            | モデルの作成バージョンです                                                                                  |
| author             | モデルの作者の名前を記述します                                                                              |
| contactInformation | モデルの作者への連絡先を記述します                                                                          |
| reference          | 何か「親作品」に相当するものがある場合は参照URLなどを記述します                                             |
| thumbnailImage     | gltf.images への index。アバターモデルのサムネイルを登録します。2048x2048程度の解像度テクスチャを推奨します |

#### アバターの人格に関する許諾範囲 
##### アバターに人格を与えることの許諾範囲

`extensions.VRMC_vrm.meta.allowedUser`

| 名前                     | 備考                                               |
|:-------------------------|:---------------------------------------------------|
| OnlyAuthor               | アバターを操作することはアバター作者にのみ許される |
| ExplicitlyLicensedPerson | 明確に許可された人限定                             |
| Everyone                 | 全員に許可                                         |

##### このアバターを用いて暴力表現を演じることの許可

`extensions.VRMC_vrm.meta.violentUsage`

| 名前     | 備考 |
|:---------|:-----|
| Disallow |      |
| Allow    |      |

##### このアバターを用いて性的表現を演じることの許可

`extensions.VRMC_vrm.meta.sexualUsage`

| 名前     | 備考 |
|:---------|:-----|
| Disallow |      |
| Allow    |      |

##### 商用利用の許可

`extensions.VRMC_vrm.meta.commercialUsage`

| 名前     | 備考 |
|:---------|:-----|
| Disallow |      |
| Allow    |      |

##### extensions.VRMC_vrm.meta.otherPermissionUrl

* 上記許諾条件以外のライセンス条件がある場合はそのライセンス文書へのURLを記述

#### 再配布・改変に関する許諾範囲

##### ライセンスタイプ

`extensions.VRMC_vrm.meta.license`

| 名前                      | 備考                                   |
|:--------------------------|:---------------------------------------|
| Redistribution Prohibited | 再配布禁止                             |
| CC0                       | 著作権放棄                             |
| CC_BY                     | Creative Commons CC BYライセンス       |
| CC_BY_NC                  | Creative Commons CC BY NCライセンス    |
| CC_BY_SA                  | Creative Commons CC BY SAライセンス    |
| CC_BY_NC_SA               | Creative Commons CC BY NC SAライセンス |
| CC_BY_ND                  | Creative Commons CC BY NDライセンス    |
| CC_BY_NC_ND               | Creative Commons CC BY NC NDライセンス |
| Other                     | その他                                 |

##### extensions.VRMC_vrm.meta.otherLicenseUrl

上記許諾条件以外のライセンス条件がある場合はそのライセンス文書へのURLを記述

### ヒューマノイド

`extensions.VRMC_vrm.humanoid`

VRM は、人間型モデルの仕様を定めています。
ここでは、GLTFのノードヒエラルキーの人間型ボーンの部分を特にヒューマノイド・スケルトンと呼びます。

#### ヒューマノイド・スケルトン仕様

* 同じヒューマノイド・ボーンはひとつずつで重複しない
* すべての必須ボーンが含まれている
* ボーンの間にボーンではないノードが入ることは許容します(LowerLegの親がemptyでその親がUpperLegであるなど)
* `向き` は、TPose時に守ることが推奨される位置関係です。 アプリケーションでボーンの向きを判定するときにトラブルの原因になりやすいので、親と子のボーンを同じまたは非常に近い座標にすることは非推奨です(浮動小数点で有効な距離を離してください)。

#### ヒューマノイド・ボーン(enum)

##### ボーンの傾き

ボーンの傾きを算出できるようにT-Poseに固定軸を追加します。
T-Pose化機能を実装している場合に固定軸を考慮することが推奨されます(オプション)。

インポート後の軸復旧例

```cs
// 計算方法
var forward = (tail - head).normalized; // ボーンの進行方向
var axis = new Vector3(1, 0, 0); // spineの場合。ボーンごとに固定(後続の表、固定軸)
var cross = Vector3.cross(forward, axis); // 外積で直角な軸を計算できます

// forward, axis, crossを軸x+-, y+-, z+-のどれに割り当てるかはシステム依存です
var y = forward;
var x = axis;
var z = cross;
```

##### 表見出しの説明

| 軸         | 備考                                                                                                                                             |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| 必須       | 必須であるボーンが無いと、ヒューマノイドとして成立しません。エラーになります                                                                     |
| 親ボーン   | nodeを親に辿っていくと最初にみつかるヒューマノイドボーンです。必須ボーンでない親が見つからない場合は、その親に遡ってください                     |
| 相対位置   | T-Pose時の親ボーンからの相対位置の方向性です。まっすぐである必要ありません。親ボーンと同じ位置もしくは極めて近い位置はエラになる可能性があります |
| 固定軸     | T-Pose時にボーンの軸がこの軸と重なるようにすると、インポート後に関節が曲がる方向のヒントになります                                               |
| 位置の目安 | ボーンの位置のだいたいの目安です                                                                                                                 |

必須欄に `(tail)` とあるのは、末尾ボーンです。
必須ではありません。
モデルにギズモのようなGUIを作るときの追加のヒントになります。
通常の、`FK` のモーションを適用する場合には関係ありません。

##### 胴(enum)

| ボーン名前 | 必須 | 親ボーン   | 相対位置       | 固定軸   | 位置の目安 | 備考                                                   |
|:-----------|:-----|:-----------|----------------|----------|------------|--------------------------------------------------------|
| hips       | 必須 |            | (親ボーン無し) | World;X+ | 股間       | 通常、このボーンだけが移動し他のボーンは回転だけします |
| spine      | 必須 | hips       | World;Y+       | World;X+ | 骨盤の上端 |                                                        |
| chest      |      | spine      | World;Y+       | World;X+ | 胸郭の下端 | 0.X では必須だった                                     |
| upperChest |      | chest      | World;Y+       | World;X+ |            |                                                        |
| neck       |      | upperChest | World;Y+       | World;X+ | 首の付け根 | 0.X では必須だった                                     |

* 骨盤に相当するボーンがhips から `Y-` 向きになっている場合がありますが非推奨です(inverted pelvis)

##### 頭(enum)

| ボーン名前 | 必須   | 親ボーン | 相対位置 | 固定軸   | 位置の目安 | 備考                                 |
|:-----------|:-------|:---------|----------|----------|------------|--------------------------------------|
| head       | 必須   | neck     | World;Y+ | World;X+ | 首の上端   |                                      |
| headTail   | (tail) | head     | World;Y+ | (tail)   | 頭の上端   |                                      |
| leftEye    |        | head     | (無し)   | (無し)   |            | 目線をボーンで動かすモデル(正面向き) |
| rightEye   |        | head     | (無し)   | (無し)   |            | 目線をボーンで動かすモデル(正面向き) |
| jaw        |        | head     | (無し)   | (無し)   |            |                                      |

##### 脚(enum)

| ボーン名前    | 必須   | 親ボーン      | 相対位置   | 固定軸   | 位置の目安 | 備考       |
|:--------------|:-------|:--------------|------------|----------|------------|------------|
| leftUpperLeg  | 必須   | hips          | World;X+   | World;X- | 脚の付け根 | hips左側。 |
| leftLowerLeg  | 必須   | leftUpperLeg  | World;Y-   | World;X- | 膝         |            |
| leftFoot      | 必須   | leftLowerLeg  | World;Y-   | World;X- | 足首       |            |
| leftToes      |        | leftFoot      | World;Y-Z+ | World;X- |            |            |
| leftToesTail  | (tail) | leftToes      | World;Z+   | (tail)   | つま先     |            |
| rightUpperLeg | 必須   | hips          | World;X-   | World;X- | 脚の付け根 |            |
| rightLowerLeg | 必須   | rightUpperLeg | World;Y-   | World;X- | 膝         |            |
| rightFoot     | 必須   | rightLowerLeg | World;Y-   | World;X- | 足首       |            |
| rightToes     |        | rightFoot     | World;Y-Z+ | World;X- |            |            |
| leftToesTail  | (tail) | rightToes     | World;Z+   | (tail)   | つま先     |            |

##### 腕(enum)

| ボーン名前    | 必須 | 親ボーン      | 相対位置 | 固定軸   | 位置の目安   | 備考 |
|:--------------|:-----|:--------------|----------|----------|--------------|------|
| leftShoulder  |      | chest         | World;X- | World;Y+ |              |      |
| leftUpperArm  | 必須 | leftShoulder  | World;X- | World;Y+ | 上腕の付け根 |      |
| leftLowerArm  | 必須 | leftUpperArm  | World;X- | World;Y+ | 肘           |      |
| leftHand      | 必須 | leftLowerArm  | World;X- | World;Z- | 手首         |      |
| rightShoulder |      | chest         | World;X+ | World;Y+ |              |      |
| rightUpperArm | 必須 | rightShoulder | World;X+ | World;Y+ | 上腕の付け根 |      |
| rightLowerArm | 必須 | rightUpperArm | World;X+ | World;Y+ | 肘           |      |
| rightHand     | 必須 | rightLowerArm | World;X+ | World;Z+ | 手首         |      |

##### 指(enum)

TODO: 親指の仕様を検討

| ボーン名前              | 必須   | 親ボーン                | 相対位置   | 固定軸       | 位置の目安 | 備考 |
|:------------------------|:-------|:------------------------|------------|--------------|------------|------|
| leftThumbProximal       |        | leftHand                | World;+X+Z | World;Y+(仮) |            |      |
| leftThumbIntermediate   |        | leftThumbProximal       | World;+X+Z | World;Y+(仮) |            |      |
| leftThumbDistal         |        | leftThumbIntermediate   | World;+X+Z | World;Y+(仮) |            |      |
| leftThumbTail           | (tail) | leftThumbDistal         | World;+X+Z | (tail)       |            |      |
| leftIndexProximal       |        | leftHand                | World;+X   | World;Z+     |            |      |
| leftIndexIntermediate   |        | leftIndexProximal       | World;+X   | World;Z+     |            |      |
| leftIndexDistal         |        | leftIndexIntermediate   | World;+X   | World;Z+     |            |      |
| leftIndexTail           | (tail) | leftIndexDistal         | World;+X   | (tail)       |            |      |
| leftMiddleProximal      |        | leftHand                | World;+X   | World;Z+     |            |      |
| leftMiddleIntermediate  |        | leftMiddleProximal      | World;+X   | World;Z+     |            |      |
| leftMiddleDistal        |        | leftMiddleIntermediate  | World;+X   | World;Z+     |            |      |
| leftMiddleTail          | (tail) | leftMiddleDistal        | World;+X   | (tail)       |            |      |
| leftRingProximal        |        | leftHand                | World;+X   | World;Z+     |            |      |
| leftRingIntermediate    |        | leftRingProximal        | World;+X   | World;Z+     |            |      |
| leftRingDistal          |        | leftRingIntermediate    | World;+X   | World;Z+     |            |      |
| leftRingTail            | (tail) | leftRingDistal          | World;+X   | (tail)       |            |      |
| leftLittleProximal      |        | leftHand                | World;+X   | World;Z+     |            |      |
| leftLittleIntermediate  |        | leftLittleProximal      | World;+X   | World;Z+     |            |      |
| leftLittleDistal        |        | leftLittleIntermediate  | World;+X   | World;Z+     |            |      |
| leftLittleTail          | (tail) | leftLittleDistal        | World;+X   | (tail)       |            |      |
| rightThumbProximal      |        | rightHand               | World;-X+Z | World;Y-(仮) |            |      |
| rightThumbIntermediate  |        | rightThumbProximal      | World;-X+Z | World;Y-(仮) |            |      |
| rightThumbDistal        |        | rightThumbIntermediate  | World;-X+Z | World;Y-(仮) |            |      |
| rightThumbTail          | (tail) | rightThumbDistal        | World;-X+Z | (tail)       |            |      |
| rightIndexProximal      |        | rightHand               | World;-X   | World;Z-     |            |      |
| rightIndexIntermediate  |        | rightIndexProximal      | World;-X   | World;Z-     |            |      |
| rightIndexDistal        |        | rightIndexIntermediate  | World;-X   | World;Z-     |            |      |
| rightIndexTail          | (tail) | rightIndexDistal        | World;-X   | (tail)       |            |      |
| rightMiddleProximal     |        | rightHand               | World;-X   | World;Z-     |            |      |
| rightMiddleIntermediate |        | rightMiddleProximal     | World;-X   | World;Z-     |            |      |
| rightMiddleDistal       |        | rightMiddleIntermediate | World;-X   | World;Z-     |            |      |
| rightMiddleTail         | (tail) | rightMiddleDistal       | World;-X   | (tail)       |            |      |
| rightRingProximal       |        | rightHand               | World;-X   | World;Z-     |            |      |
| rightRingIntermediate   |        | rightRingProximal       | World;-X   | World;Z-     |            |      |
| rightRingDistal         |        | rightRingIntermediate   | World;-X   | World;Z-     |            |      |
| rightRingTail           | (tail) | rightRingDistal         | World;-X   | (tail)       |            |      |
| rightLittleProximal     |        | rightHand               | World;-X   | World;Z-     |            |      |
| rightLittleIntermediate |        | rightLittleProximal     | World;-X   | World;Z-     |            |      |
| rightLittleDistal       |        | rightLittleIntermediate | World;-X   | World;Z-     |            |      |
| rightLittleTail         | (tail) | rightLittleTail         | World;-X   | (tail)       |            |      |

#### Tポーズ仕様

スケルトンは初期姿勢としてTポーズをとっている必要があります。
TPoseの仕様は以下の通りです。

* rootボーン(hipsの祖先でもっとも根元にあるボーン)は原点
* モデルの踵が接地(Y=0)している

| モデルの部分                                                       | 方向 |
|:-------------------------------------------------------------------|:-----|
| 前                                                                 | Z-   |
| 左                                                                 | X+   |
| 上                                                                 | Y+   |
| 胴体(hips - spine - chest - upperChest - upperChest - neck - head) | Y+   |
| 左右脚(upperLeg - lowerLeg)                                        | Y-   |
| 左右足                                                             | Y-Z- |
| 左右つま先                                                         | Z-   |
| 左腕(shoulder - upperArm - lowerArm - hand)と各指                  | X+   |
| 左親指 Proximal                                                    | X+Z- |
| 右腕(shoulder - upperArm - lowerArm - hand)と各指                  | X-   |
| 右親指 Proximal                                                    | X-Z- |
| 手のひら                                                           | Y-   |

### モデルの正規化

VRMモデルのGLTF部分に対する制約と、それを実現するためのアルゴリズムです。

#### ノードの正規化

VRMのノードは以下の制約を受けます。
初期姿勢(T-Pose)で、

* 回転が無い
* スケールが無い(1, 1, 1)

#### メッシュの正規化

ノードの正規化を達成するために、メッシュを正規化する必要があります。
正規化されたメッシュはスキニングしていない状態で以下の制約を受けます

* 初期姿勢のヒューマノイド・スケルトンと重なる(skin.rootがある場合はその座標を加算する)
  * メッシュの前は Z-
  * メッシュの右は X+
  * メッシュの上は Y+

結果として、`skin.inverseBindMatrices` は、回転・スケールを含まずに対象ボーンのワールド座標に対する負の移動のみを含みます。

```js
// Inverse Bind Matrices
// [x, y, z] = bone.translation - skin.translation
[1, 0, 0, 0]
[0, 1, 0, 0]
[0, 0, 1, 0]
[-x,-y,-z, 1]
```

スキニングに関して以下の制約を受けます。

* skin.skeleton が無い、もしくは原点にあるノード

#### 正規化のアルゴリズム

* モデルをT-Poseにする
* skinなし
  * 各頂点にNode.matrixを乗算する
* skinあり
  * BoneWeightの無い頂点がもしあれば、skin.root に対しするBoneWeightを付与する
  * メッシュをスキニングして結果をメッシュとして再取り込みする(bake, freeze)
* Nodeの回転・スケールを除去する

### BlendShape

VRMは、ヒューマノイド向けに MorphTarget を拡張しています。
複数のMorphTargetをグループ化して意味(瞬き、あいうえお、喜怒哀楽)を持たせます。
また、マテリアル値(color, texture offset+scale)を変化させることができます。

#### BlendShapePreset(enum)

##### 表情(enum)

| 名前    | 備考                                   |
|:--------|:---------------------------------------|
| neutral | 待機状態。`TODO: 廃止して、ベイクする` |
| joy     | 喜                                     |
| angry   | 怒                                     |
| sorrow  | 哀                                     |
| fun     | 楽                                     |

##### リップシンク(enum)

| 名前 | 備考 |
|:-----|:-----|
| aa   | あ   |
| ih   | い   |
| ou   | う   |
| ee   | え   |
| oh   | お   |

##### 瞬き(enum)

| 名前       | 備考             |
|:-----------|:-----------------|
| blink      | 両方の瞼を閉じる |
| blinkLeft  | 左瞼を閉じる     |
| blinkRight | 右瞼を閉じる     |

##### 視線(enum)

| 名前      | 備考                                                                   |
|:----------|:-----------------------------------------------------------------------|
| lookUp    | ボーンではなくBlendShapeで視線が動くモデル向け。[視線制御](#視線制御)で詳述 |
| lookDown  | ボーンではなくBlendShapeで視線が動くモデル向け。[視線制御](#視線制御)で詳述 |
| lookLeft  | ボーンではなくBlendShapeで視線が動くモデル向け。[視線制御](#視線制御)で詳述 |
| lookRight | ボーンではなくBlendShapeで視線が動くモデル向け。[視線制御](#視線制御)で詳述 |

##### ユーザー定義(enum)

| 名前   | 備考                                                                                    |
|:-------|:----------------------------------------------------------------------------------------|
| custom | Applicationが独自に決めたBlendShapeを使う場合などに使用します。nameで識別してください。 |

#### BlendShapeの仕様

`extensions.VRMC_vrm.blendshape`

| 名前                             | 備考                                                                                                         |
|:---------------------------------|:-------------------------------------------------------------------------------------------------------------|
| blendShapeGroups[*].preset       | 対象のBlendShapePreset                                                                                       |
| blendShapeGroups[*].name         | 任意の名前(ユニークかつファイル名で使える文字のみ)                                                           |
| blendShapeGroups[*].is_binary    | trueの場合 value!=0 を 1 とみなします                                                                        |
| blendShapeGroups[*].values       | BlendShapeBind(後述) のリスト                                                                                |
| blendShapeGroups[*].materials    | MaterialValueBind(後述) のリスト                                                                             |
| blendShapeGroups[*].ignoreBlink  | このBlendShapeのWeightが0でないときに、blink, blink_L, blink_R のウェイトを強制的に0にします。               |
| blendShapeGroups[*].ignoreLookAt | このBlendShapeのWeightが0でないときに、lookUp, lookDown, lookLeft, lookRight のウェイトを強制的に0にします。 |
| blendShapeGroups[*].ignoreMouth  | このBlendShapeのWeightが0でないときに、A, I, U, E, O のウェイトを強制的に0にします。                         |

##### BlendShapeBind

`extensions.VRMC_vrm.blendshape[*].binds[*]`

BlendShapeと MorphTarget を結びつけます。

| 名前   | 備考                                                                                          |
|:-------|:----------------------------------------------------------------------------------------------|
| mesh   | 対象meshのindex                                                                               |
| index  | 対象morphのindex(すべてのprimitiveが同じmorphを持ちます[Meshの格納の制約](#Meshの格納の制約)) |
| weight | 適用したときのmorph値                                                                         |

##### MaterialValueBind

`extensions.VRMC_vrm.blendshape[*].materialValues[*]`

BlendShapeと Material の変化を結びつけます。

| 名前        | 備考                                             |
|:------------|:-------------------------------------------------|
| material    | 対象のmaterialのindex                            |
| type        | materialの変更対象項目(color, uvScale, uvOffset) |
| targetValue | 適用したときのmaterial値(float4)                 |

`extensions.VRMC_vrm.blendshape[*].materialValues[*].type`

| 名前     | 備考                                        |
|:---------|:--------------------------------------------|
| color    | mainColor を変化させます                    |
| uvScale  | テクスチャーのUVパラメーター を変化させます |
| uvOffset | テクスチャーのUVパラメーター を変化させます |

#### BlendShape更新のアルゴリズム

##### BlendShapeの識別

* presetがcustom以外の場合は、presetで識別
* customの場合は、nameで識別

##### MorphTarget

* すべてのMorphTargetが0の状態にする
* 任意のBlendShapeの値(Weight)を加算する `void AccumulateValue(BlendShapeClip clip, float value)`
* 加算された値をすべて適用する

##### Material値

* すべてのMaterialが初期状態にする(0ではなく)
* 任意のBlendShapeの値(Weight)を加算する `void AccumulateValue(BlendShapeClip clip, float value)`
* 加算された値をすべて適用する `Base + (A.Target - Base) * A.Weight + (B.Target - Base) * B.Weight`

### 一人称

VRMは、VRを想定した一人称視点の設定を定義しています。
lookAt.offsetFromHeadBone を HMDの参考位置に利用できます。

#### MeshAnnotation

Mesh単位での描画を制御します。

`extensions.VRMC_vrm.firstPerson.meshAnnotations[*]`

| 名前            | 備考              |
|:----------------|:------------------|
| mesh            | 対象meshへのindex |
| firstPersonFlag | 後述              |

firstPersonFlag。VRアプリでモデルを使用した場合に、自モデルの描画をHMDとそれ以外で切り分けます。

#### MeshAnnotation(enum)

`thirdPersonOnly` は、VR描画時に自分の頭部を非表示にすることを意図しています。VR視点カメラは、頭部の中に位置することが想定されます。そのため、顔・頭・髪のポリゴンが視界に見えてしまうのを防ぎます。

特に描き分ける必要ないオブジェクトは、 `both` にして、すべてのカメラから描画されるようにします。

`firstPersonOnly` は、自分だけに見えて他人視点からは見えない設定です。アプリ側で特別な用途が無ければ使いません。

`auto` はMeshを頭部(`thirdPersonOnly`)とその他(`both`)に分割してください。次の節で詳述します。

| 名前            | 1人称カメラ(VR視点) | その他のカメラ | 例                                                                 |
|:----------------|:--------------------|:---------------|--------------------------------------------------------------------|
| thirdPersonOnly | 描画しない          | 描画する       | 顔・目・頭・髪、帽子・ヘルメットなどVR視界を遮るもの               |
| firstPersonOnly | 描画する            | 描画しない     | プレイヤーだけに見えてたの視点からは見えないユーザーインタフェース |
| both            | 描画する            | 描画する       |                                                                    |

#### MeshAnnotation.Autoのアルゴリズム

* Meshの頂点をすべて調べて、Headボーンとその子孫のボーンに対するWeightを持つ頂点を集める
* 上記の頂点を含む三角形と含まない三角形に２分したMeshを作成する
* 上記の頂点を含むMeshをThirdPersonOnly、含まないMeshをBothとしたに設定する

### 視線制御

`extensions.VRMC_vrm.lookAt`

VRMは、ヒューマノイド向けに視線制御を定義しています。

| 名前               | 備考                                                                 |
|:-------------------|:---------------------------------------------------------------------|
| lookAtType         | bone または blendShape                                               |
| offsetFromHeadBone | lookAtの基準位置(両目の間が目安)へのヘッドボーンからの位置offsetです |
| horizontalInner    | 水平内側の目の可動範囲                                               |
| horizontalOuter    | 水平外側の目の可動範囲                                               |
| verticalDown       | 下方向の目の可動範囲                                                 |
| verticalUp         | 上方向の目の可動範囲                                                 |

#### LookAtType

| 名前            | 備考                                                              |
|:----------------|:------------------------------------------------------------------|
| bone            | leftEyeボーンとrightEyeボーンで視線制御します                     |
| blendShape      | BlendShapeのLookAt, LookDown, LookLeft, LookRightで視線制御します |

blendShape型はさらに、morph型とuv型を設定できます(BlendShapeの設定)。

#### 水平内外、垂直上下

目の可動範囲を調整します。

##### 水平内側

`extensions.VRMC_vrm.lookAt.horizontalInner`

* 左目の右方向
* 右目の左方向
* boneタイプ: outputScale には leftEye, rightEye ボーンの Euler 角(radian) による最大回転角度を指定します
* blendShapeタイプ: outputScale には LookLeft, LookRight BlendShape の最大適用量を指定します(最大1.0)

```
Y = clamp(yaw, 0, horizontalInner.inputMaxValue)/horizontalInner.inputMaxValue * horizontalInner.outputScale 
```

##### 水平外側

`extensions.VRMC_vrm.lookAt.horizontalOuter`

* 左目の左方向
* 右目の右方向
* boneタイプ: outputScale には leftEye, rightEye ボーンの Euler 角(radian) による最大回転角度を指定します
* blendShapeタイプ: outputScale には LookLeft, LookRight BlendShape の最大適用量を指定します(最大1.0)

```
Y = clamp(yaw, 0, horizontalOuter.inputMaxValue)/horizontalOuter.inputMaxValue * horizontalOuter.outputScale 
```

##### 垂直下方向

`extensions.VRMC_vrm.lookAt.verticalDown`

* 左目の下方向
* 右目の下方向
* boneタイプ: outputScale には leftEye, rightEye ボーンの Euler 角(radian) による最大回転角度を指定します
* blendShapeタイプ: outputScale には LookLeft, LookRight BlendShape の最大適用量を指定します(最大1.0)

```
Y = clamp(yaw, 0, verticalDown.inputMaxValue)/verticalDown.inputMaxValue * verticalDown.outputScale 
```

##### 垂直上方向

`extensions.VRMC_vrm.lookAt.verticalUp`

* 左目の上方向
* 右目の上方向
* boneタイプ: outputScale には leftEye, rightEye ボーンの Euler 角(radian) による最大回転角度を指定します
* blendShapeタイプ: outputScale には LookLeft, LookRight BlendShape の最大適用量を指定します(最大1.0)

```
Y = clamp(yaw, 0, verticalUp.inputMaxValue)/verticalUp.inputMaxValue * verticalUp.outputScale 
```

##### ボーンタイプ

可動範囲調整後の Yaw, Pitch 角の値をオイラー角として leftEye, rightEye ボーンの LocalRotation に適用します。

##### BlendShapeタイプ

可動範囲調整後の Yaw, Pitch 角をBlendShapeのweight値として LookLeft, LookRight, LookDown, LookUp BlendShapeに適用します。

#### LookAtのアルゴリズム

* HeadBoneのforwardベクトルと、HeadBone + firstPersonBoneOffset の位置から注視点を結んだベクトルが為す方向の Yaw Pitch 角を計算
* x, y = ClampAndScale(yaw, pitch)
* apply(x, y)

##### Boneタイプ

| bone と yaw, pitch              | 備考                                                 |
|:--------------------------------|:-----------------------------------------------------|
| leftEye + yaw(左)               | horizontalOuter を適用してオイラー角として反映します |
| leftEye + yaw(右)               | horizontalInner を適用してオイラー角として反映します |
| rightEye + yaw(左)              | horizontalOuter を適用してオイラー角として反映します |
| rightEye + yaw(右)              | horizontalInner を適用してオイラー角として反映します |
| leftEye or rightEye + pitch(下) | verticalDown を適用してオイラー角として反映します    |
| leftEye or rightEye + pitch(上) | verticalUp を適用してオイラー角として反映します      |

##### BlendShapeタイプ

| bone と yaw, pitch              | 備考                                                                 |
|:--------------------------------|:---------------------------------------------------------------------|
| leftEye + yaw(左)               | horizontalOuter を適用して BlendShape LookLeft の値として反映します  |
| leftEye + yaw(右)               | horizontalInner を適用して BlendShape LookRight の値として反映します |
| rightEye + yaw(左)              | horizontalOuter を適用して BlendShape LookLeft の値として反映します  |
| rightEye + yaw(右)              | horizontalInner を適用して BlendShape LookRight の値として反映します |
| leftEye or rightEye + pitch(下) | verticalDown を適用して BlendShape LookDown の値として反映します     |
| leftEye or rightEye + pitch(上) | verticalUp を適用して BlendShape LookDown の値として反映します       |

LookAtのBlendShapeタイプは、 MorphTarget タイプと TextureUVOffset タイプがありますが、ここでの処理は同じです。

### SpringBone

VRMは、Physicsエンジンによらない揺れものを定義しています。
髪や衣装が揺れるような見た目専用を想定しています。

| 名前           | 備考                  |
|:---------------|:----------------------|
| boneGroups     | SpringBoneのリスト    |
| colliderGroups | ColliderGroupのリスト |

#### SpringBone

| 名前           | 備考                                           |
|:---------------|:-----------------------------------------------|
| comment        | 自由文字列                                     |
| stiffness      | 剛性。硬さ。初期姿勢に戻ろうとする力           |
| gravityPower   | 重力加速度                                     |
| gravityDir     | 重力方向。下以外に重力を働かせることができます |
| dragForce      | 空気抵抗。減速する力                           |
| center         | Local座標となるNodeのindex                     |
| hitRadius      | SpringBoneの当たり判定の半径                   |
| bones          | SpringBoneを設定するNodeのindexの配列          |
| colliderGroups | 当たり判定を処理する colliderGroup のindex     |

#### ColliderGroup

| 名前      | 備考                      |
|:----------|:--------------------------|
| node      | Colliderの設置Nodeのindex |
| colliders | Colliderのリスト          |

#### Collider

* SpringBone専用で、Physicsシステムとは独立していることを想定しています。

| 名前      | 備考                                                                        |
|:----------|:----------------------------------------------------------------------------|
| shapeType | Colliderの形状。sphere, capsule                                             |
| offset    | ColliderのNodeからのローカル位置                                            |
| rotation  | ColliderのNodeからのローカル回転。Euler角Radians                            |
| size      | Colliderの半径。sphereのときは {radians}, capsuleのときは {radians, length} |

#### SpringBoneのアルゴリズム

TODO:

```cs
// verlet 積分で次の位置を計算
var nextTail = currentTail
    + (currentTail - prevTail) * (1.0f - dragForce) // 前フレームの移動を継続する(減衰もあるよ)
    + ParentRotation * m_localRotation * m_boneAxis * stiffnessForce // 親の回転による子ボーンの移動目標
    + external // 外力による移動量
    ;
```

### Material

トゥーンシェーダーを定義しています。

* require VRMC_materials_mtoon extension

アンライトが必須です。

* require KHR_materials_unlit extension

テクスチャトランスフォーム。
BlendShapeによるアニメーション定義があります。

* require KHR_texture_transform extension

### Constraint

TODO:

### 更新の適用順 

推奨される更新の適用順です。

1. ヒューマノイド・ボーンを解決
2. 頭の位置が決まるのでLookAtを解決
  * Bone型
  * BlendShape型
3. ブレンドシェイプUpdate
  * LipSync
  * AutoBlink
  * BlendShape型のLookAt
  * コントローラーなど外部入力
4. ブレンドシェイプApplyする
5. コンストレイントを解決
6. SpringBoneを解決

## glTF Schema Updates

### 座標の単位

glTFの [coordinate-system-and-units](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0#coordinate-system-and-units) に準拠してメートル単位です。

### 使わない項目

以下の項目を使用しません。

* animations
* cameras

### 各種名前の制約

* ユニークにする
* ファイル名に使える文字にする

#### 名前のエスケープ例

```cs
static readonly char[] EscapeChars = new char[]
{
    '\\',
    '/',
    ':',
    '*',
    '?',
    '"',
    '<',
    '>',
    '|',
};
public static string EscapeFilePath(this string path)
{
    foreach(var x in EscapeChars)
    {
        path = path.Replace(x, '+');
    }
    return path;
}
```

### Meshの格納の制約

#### TANGENTを保存しない

* TANGENT を保存しない。NormalMapが存在する場合は、MikkTSpaceアルゴリズムで計算すること

#### MorphTargetに法線とTangentを保存しない

#### morphTarget名

* meshes[*].primitives[*].extras.targetNames にモーフターゲットの名前を格納

#### VertexBufferの構成

ある meshの primitive 毎に異なるバッファ構成を禁止しています。

* あるmeshの各 primitive は同じ attributes を持たなければならない

primitive 毎に独立した morph を禁止します。
primitive ではなく mesh に対して morph targets を設定することを想定しています。

* 各 primitive は同じ targets を持たなければならない
* 各 primitive.targets は同じ attributes を持たなければならない

##### 推奨(共有バッファ方式)

ひとつのVertexBuffer(accessor)を複数の primitive.indices から 参照する。

```
primitives[*].attributes(各primitiveで同一のaccessorを使う)
 +----------------------------------+
 |(0)                               |POSITION
 +----------------------------------+
 +----------------------------------+
 |(1)                               |NORMAL
 +----------------------------------+
 +----------------------------------+
 |(2)                               |TEXCOORD_0
 +----------------------------------+
      ^         ^            ^
      |         |            |
primitives      |            |
  [0].indices   |            |
 +-----------+  |            |
 |(3)        |  |            |
 +-----------+  |            |
              [1].indices    |
             +------------+  |
             |(4)         |  |
             +------------+  |
                           [2].indices
                          +-----------+
                          |(5)        |
                          +-----------+

box: accessor
```

* VertexBufferがひとつなので、すべてのprimitiveで同じ attributes になることを強制できる
* primitive.targets も同様

##### GLTF標準(分割バッファ方式)

```
primitives[*].attributes(各primitiveで独自のaccessorを使う)
 +-----------+ +----------+ +----------+
 |(0)        | |(3)       | |(6)       |POSITION
 +-----------+ +----------+ +----------+
 +-----------+ +----------+ +----------+
 |(1)        | |(4)       | |(7)       |NORMAL
 +-----------+ +----------+ +----------+
 +-----------+ +----------+ +----------+
 |(2)        | |(5)       | |(8)       |TEXCOORD_0
 +-----------+ +----------+ +----------+
      ^         ^            ^
      |         |            |
primitives
  [0].indices   [1].indices  [2].indices
 +-----------+ +----------+ +----------+
 |(9)        | |(10)      | |(11)      |indices
 +-----------+ +----------+ +----------+

box: accessor
```

* すべての primitive が同じattributesを持つようにする
  * 例: `primitive[0]`(POSITION, NORMAL, TEXCOORD_0)に対し `primitive[1]`(POSITION) と構成が違う可能性がある
* primitive.targets も同様

```
NG

primitives[*].attributes(各primitiveで独自のaccessorを使う)
 +-----------+ +----------+ +----------+
 |(0)        | |(3)       | |(6)       |POSITION
 +-----------+ +----------+ +----------+
 +-----------+ +----------+             
 |(1)        | |(4)       |             NORMAL
 +-----------+ +----------+             
 +-----------+                          
 |(2)        |                          TEXCOORD_0
 +-----------+                          
      ^         ^            ^
      |         |            |
primitives
  [0].indices   [1].indices  [2].indices
 +-----------+ +----------+ +----------+
 |(9)        | |(10)      | |(11)      |indices
 +-----------+ +----------+ +----------+

box: accessor
```

## JSON Schema

```js
{
  "extensionsUsed": {
    "VRMC_vrm"
  },
  "extensions": {
    "VRMC_vrm": {
      // VRMの拡張情報
      "specVersion": "1.0"
    }
  },
  // 通常のGLTF-2.0の情報
}
```

* https://github.com/vrm-c/UniVRM/tree/master/specification/1.0/schema

GLTF-2.0のJsonSchema

* https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/schema

## Known Implementations

### VRM-0.0

* https://github.com/vrm-c/UniVRM
* https://github.com/virtual-cast/babylon-vrm-loader
* https://github.com/pixiv/three-vrm
* https://github.com/iCyP/VRM_IMPORTER_for_Blender2_8

## Resources

* https://vrm.dev/
