# Information Delivery Specification（情報提供仕様）

<img src="Graphics/IDS-logo-with-letters.png" alt="IDS Logo" width="300"/>

**Information Delivery Specification (IDS)（情報提供仕様）** は、IFCモデルからのシンプルな情報要件を指定・チェックするためのbuildingSMART標準です。モデルチェックのための無料で軽量な標準化されたアプローチとして設計されています。詳細は[公式ウェブサイト](https://www.buildingsmart.org/standards/bsi-standards/information-delivery-specification-ids/)をご覧ください。

## はじめに

IDSは、情報**Specifications（要件）** のリストを含む`.ids`拡張子で終わるファイル形式です。例えば、単一の**Specification（要件）** は「_すべての壁に防火等級プロパティが必要である_」と言うかもしれません。IDSファイルを受け取るモデル作成者は、各**Specification（要件）** に対して必要な情報がすべて提供されていることを確認するためにそれを使用できます。モデル受領者は、IFCモデルがすべての**Specifications（要件）** を満たしているかどうかをチェックするためにIDSファイルを使用できます。**Specification（要件）** コンプライアンスチェックの結果をリストするレポートも生成できます。

![IDS Diagram](Graphics/ids-diagram.png)

IDSファイル作成ツールとモデルチェックツールは、多くの[ソフトウェアベンダー](https://technical.buildingsmart.org/ids-software-implementations/)によって提供されています。任意のソフトウェアから生成されたIFCモデルは、IDSファイルに対してチェックできます。

## IDS構造

各IDSファイルは[メタデータ](ids-metadata.md)で記述でき、1つまたは複数の[specifications（要件）](specifications.md)を含むことができます。Specifications（要件）は2つの部分から構成されます：applicability（適用対象）- このSpecification（要件）の対象となる要素を記述し、requirements（要求条件）- 該当する要素が持つべき、または持つべきでないものをリストします。Applicability（適用対象）とrequirements（要求条件）の両方は、プロパティ、エンティティ、分類、材料、partOfなどのファセットで構築されます。

## 始め方

1. IDSチェックをサポートするソフトウェアを選択します（[IDSをサポートするツールのリスト](https://technical.buildingsmart.org/ids-software-implementations/)を参照）。
2. [サンプルIDSファイル](../Examples/IDS_wooden-windows.ids)をダウンロードします。
3. IDSと照合するための[サンプルIFCモデル](../Examples/IDS_wooden-windows_IFC.ifc)をダウンロードします。
4. IDSとIFCの両方をソフトウェアに読み込み、チェックプロセスを開始します。
5. すべての非準拠のレポートを取得するはずです。

以上です！より多くのサンプルIDSファイルを[Examples](../Examples)で見つけることもできます。サポートが必要な場合は、[buildingSMART Forums](https://forums.buildingsmart.org/)で遠慮なくヘルプを求めてください。

## IDSについてもっと学ぶ

1. [**Specifications（要件）** の仕組みとは？](specifications.md)
2. [優れた**Specification（要件）** メタデータの指定に関するガイドライン](ids-metadata.md)
3. [**Complex Restrictions（複雑な制限）** の指定方法を学ぶ](restrictions.md)
4. [**Entity Facet（エンティティファセット）** の使用方法を学ぶ](entity-facet.md)
5. [**Attribute Facet（基本属性ファセット）** の使用方法を学ぶ](attribute-facet.md)
6. [**Classification Facet（分類ファセット）** の使用方法を学ぶ](classification-facet.md)
7. [**Property Facet（プロパティファセット）** の使用方法を学ぶ](property-facet.md)
8. [**Material Facet（材料ファセット）** の使用方法を学ぶ](material-facet.md)
9. [**PartOf Facet（PartOfファセット）** の使用方法を学ぶ](partof-facet.md)
10. [ソフトウェア開発者の方へ？開発者ガイドをお読みください！](../ImplementersDocumentation/developer-guide.md)