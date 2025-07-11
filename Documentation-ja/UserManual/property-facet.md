以下が日本語翻訳版です：

---

# プロパティファセット

IFC **Properties** は、IFC内のオブジェクトにデータを付与する最も一般的な方法であり、おそらく最も使用頻度の高いIDSファセットです。

**Properties** は、"FireRating"のような名前（IDSでは**BaseName**）で識別され、類似するテーマ別に整理するのに役立つ"Pset_WallCommon"のような**Property Sets**にグループ化されます。IFC Propertiesは**Values**を持ち、これらは特定の型であり、関連する場合は単位を表現します。

buildingSMARTは、名前、セット、データ型を義務付けることで、シームレスなデータ交換を支援するための標準化された**Property Sets**と**Properties**を提供しています。例：

|baseName|propertySet|dataType|
|---|---|---|
|ThermalTransmittance|Pset_WallCommon|IFCTHERMALTRANSMITTANCEMEASURE|
|FireRating|Pset_WallCommon|IFCLABEL|
|Length|Qto_WallBaseQuantities|IFCLENGTHMEASURE|

ユーザーはカスタムの**Properties**と**Property Sets**を定義することもでき、これらはプロジェクト固有のものや、IFCの**Property Set**テンプレート機能を使用して配布されるものがあります。当然、カスタムなものを発明する前に、buildingSMARTによって標準化された**Properties**を要求することが推奨されます。

すべての標準化された**Property Sets**は、予約されたプレフィックス"Pset_"または"Qto_"で始まります。これらのプレフィックスは、カスタムプロパティには使用禁止です。

標準化された**Properties**は異なるエンティティに適用されます。例えば、**LoadBearing**のような一部のプロパティは、壁、柱、梁に適用できますが、家具、ダクト、ケーブルには適用できません。 これは**Applicable Entity**（適用対象エンティティ）として知られています。 IDSで**Properties**を指定する際は、どのオブジェクトに適用できるかを考慮することが重要です。 あらゆる種類のオブジェクトに**Properties**を適用できます。ドア、窓、スラブなどの物理的オブジェクトだけでなく、タスク、材料、構造プロファイル断面、労働リソースなどの非物理的オブジェクトにも適用できます。

**Quantity**として知られる特別な種類の**Property**があります。**Properties**がオブジェクトに関する任意の情報を指すのに対し、**Quantities**は特にオブジェクトの計算された寸法、例えば長さ、幅、高さ、表面積、正味体積を指します。 IFCは**Properties**と**Quantities**を区別していますが、IDSでは互換性があり、このファセットを使用して**Properties**と同じように**Quantities**を指定することができます。 **Properties**と同様に、**Quantities**は**Quantity Sets**にグループ化され、**Value**を持ちます。

buildingSMARTによって標準化された**Properties**を確認するには、以下のリストをチェックしてください。 **Property Sets**のリストが表示されます。**Property Set**をクリックすると、そのページに移動し、 ページタイトルの直下に**Applicable Entity**が表示され、**Property Names**と**Values**の期待されるデータ型のテーブルが示されます。これらは**Applicable Entity**を持ちます。

- [IFC4X3_ADD2 Property and Quantity Sets](http://ifc43-docs.standards.buildingsmart.org/IFC/RELEASE/IFC4x3/HTML/annex-b3.html)
- [IFC4 Property Sets](https://standards.buildingsmart.org/IFC/RELEASE/IFC4/ADD2_TC1/HTML/link/alphabeticalorder-property-sets.htm)
- [IFC4 Quantity Sets](https://standards.buildingsmart.org/IFC/RELEASE/IFC4/ADD2_TC1/HTML/link/alphabeticalorder-quantity-sets.htm)
- [IFC2X3 Property Sets](https://standards.buildingsmart.org/IFC/RELEASE/IFC2x3/TC1/HTML/psd/psd_index.htm)

注意：IFC2X3には、buildingSMARTの標準化されたプロパティのみがあり、数量はありません。

ドキュメントを確認する代わりに、IDS作成ソフトウェアが有効な**Property Sets**の候補を絞り込むのに役立つ場合があります。

## サポートされているプロパティの種類

IFCには様々な種類のプロパティがあります。IDSは、シンプルな[単一値](https://ifc43-docs.standards.buildingsmart.org/IFC/RELEASE/IFC4x3/HTML/lexical/IfcPropertySingleValue.htm)、[範囲制限値](https://ifc43-docs.standards.buildingsmart.org/IFC/RELEASE/IFC4x3/HTML/lexical/IfcPropertyBoundedValue.htm)、[リスト](https://ifc43-docs.standards.buildingsmart.org/IFC/RELEASE/IFC4x3/HTML/lexical/IfcPropertyListValue.htm)、[テーブル](https://ifc43-docs.standards.buildingsmart.org/IFC/RELEASE/IFC4x3/HTML/lexical/IfcPropertyTableValue.htm)、[列挙型](https://ifc43-docs.standards.buildingsmart.org/IFC/RELEASE/IFC4x3/HTML/lexical/IfcPropertyEnumeratedValue.htm)の指定を可能にしますが、[~~複雑プロパティ~~](https://ifc43-docs.standards.buildingsmart.org/IFC/RELEASE/IFC4x3/HTML/lexical/IfcComplexProperty.htm)と[~~参照値~~](https://ifc43-docs.standards.buildingsmart.org/IFC/RELEASE/IFC4x3/HTML/lexical/IfcPropertyReferenceValue.htm)はIDSでサポートされていません。

リスト、テーブル、範囲制限、列挙型プロパティの解釈は、次のようにIDS要求条件に応じて変化します：

- IDS値が単一値の場合、IFC値の少なくとも1つが一致する必要があります。
- IDS値が制限（minExclusive、maxExclusive、minInclusive、maxInclusiveを含む）の場合、すべてのIFC値が範囲を満たす必要があります。

IFC範囲制限値プロパティは一端のみ制限できるため、IDS要求条件を満たすには、IDS制限範囲内に完全に収まる必要があります。例：

|IDS値|IFC下限|IFC上限|期待される結果|理由|
|:-:|:-:|:-:|:-:|---|
|>2 and ≤5|3|4|✔️|下限と上限の両方が指定範囲内にある|
|>2 and ≤5||4|❌|下限が制限の最小値を下回る可能性がある|
|>2 and ≤5|3||❌|上限が制限の最大値を上回る可能性がある|
|>2 and ≤5|2|3|❌|制限が最小範囲を排他的であるため、下限が無効|
|>2 and ≤5|3|5|✔️|制限が最大範囲を包含的であるため、上限が有効|
|3|2|4|✔️|下限と上限が指定値を含む|
|2|2|4|✔️|下限が指定値と一致する|
|2|2||✔️|提供された唯一の境界が指定値と互換性がある|
|2||2|✔️|提供された唯一の境界が指定値と互換性がある|
|5|2|4|❌|下限と上限が指定値を除外する|
|5||4|❌|提供された唯一の境界が指定値と互換性がない|
|>2 and ≤5|||❌|下限と上限の少なくとも一方が必要|
|3|||❌|下限と上限の少なくとも一方が必要|
||2||✔️|値の比較は行われず、少なくとも一つの値が提供される|

## プロパティのデータ型

IDSファセットでは、**Properties** は期待される格納形式を制約するデータ型を持つ場合があります（例：テキスト値、ブール値、数値）。 数値の場合、値は単位なしで、カウント値のような値であり、単位は指定された`dataType`に関連付けられた測定値に依存します。 当方の[単位ドキュメント](https://claude.ai/chat/units.md)は、受け入れ可能な測定値のリストとその表現に使用されるSI単位を提供します。詳細については、以下のリンクのIFCドキュメントを参照してください：

- [IFC4X3_ADD2 データ型](http://ifc43-docs.standards.buildingsmart.org/IFC/RELEASE/IFC4x3/HTML/annex-b2.html)
- [IFC4 データ型](https://standards.buildingsmart.org/IFC/RELEASE/IFC4/ADD2_TC1/HTML/link/alphabeticalorder-defined-types.htm)
- [IFC2X3 データ型](https://standards.buildingsmart.org/IFC/RELEASE/IFC2x3/TC1/HTML/alphabeticalorder_definedtype.htm)

便宜上、一般的なデータ型の短いリストを以下に示します：

|データ型|使用シナリオ|
|---|---|
|IFCLABEL|人間が読むことを意図した最もシンプルなテキスト値|
|IFCIDENTIFIER|コンピュータが読むことを意図した識別コード、通常はコンピュータによって生成される|
|IFCTEXT|人間が読む長い説明|
|IFCBOOLEAN|真または偽（はい/いいえ選択としても知られる）|
|IFCINTEGER|1、2、3などの任意の整数|
|IFCREAL|1、2、3.14などの任意の数値|
|IFCCOUNTMEASURE|何かの数量をカウントするために使用される整数|
|IFCLENGTHMEASURE|何かの物理的長さを測定するために使用される浮動小数点数|
|IFCAREAMEASURE|何かの物理的面積を測定するために使用される浮動小数点数|
|IFCVOLUMEMEASURE|何かの物理的体積を測定するために使用される浮動小数点数|
|IFCDATE|何かが起こるまたは起こった日付、例：2020-01-01|
|IFCDURATION|3か月、1週間、4日、1時間などの時間の期間|

IDSは現在、SI単位に基づいてすべての測定ベースの値を指定します。各データ型に指定された単位の完全なリストは、[IDS単位テーブル](https://claude.ai/chat/units.md)で確認できます。 データ型を使用して特定の測定値（例：IFCLENGTHMEASURE）を要求することはできますが、IDSを使用して特定の単位（例：メートル、インチ、ミリメートル）で長さを測定することを要求することはできません。

プロパティは、モデル内のオブジェクトに補足情報を提供する上で重要です。

データが高度に構造化され、予測可能に取得できるよう、可能な限りbuildingSMARTの標準化された**Properties**に従うことが推奨されます。

## パラメータ

|パラメータ|必須|制限許可|許可値|意味|
|---|---|---|---|---|
|**propertySet**|✔️|✔️|任意のカスタムまたはbuildingSMART標準化プロパティセット名。標準化された名前は"Pset_"または"Qto_"で始まり、IFCドキュメントで確認できます。|オブジェクトが指定されたプロパティセットを持つ。|
|**baseName**|✔️|✔️|任意のテキストプロパティ名。標準化されたbuildingSMARTプロパティ名は、buildingSMARTドキュメントで確認できます。|プロパティは指定されたプロパティセット内に存在し、空でない値を持つ必要があります。|
|**dataType**|❌|✔️|参照されるスキーマバージョンと互換性のある有効なデータ型、大文字で表現。|値は指定されたデータ型を使用する必要があります。IDSで指定される単位は[IDS単位テーブル](https://claude.ai/chat/units.md)を使用しますが、プロジェクトは任意の単位を使用できるため、プロジェクト値は比較前にSI単位に変換される必要があります。ユーザーインターフェースは、開発者またはユーザーが好む任意の単位を表示することが許可されています。|
|**value**|❌|✔️|プロパティのデータ型に適した任意の値。指定されない場合、空でない任意の値が許可されます。測定型の値は[IDS単位テーブル](https://claude.ai/chat/units.md)で定義された単位に従って格納されます|プロパティの値は一致する必要があります。詳細については[DataType documentation](https://claude.ai/ImplementersDocumentation/DataTypes.md#xml-base-types)を参照してください。|
|**uri**|❌|❌|プロパティのUniform Resource Identifier。リソースには名前と定義を含み、できればISO 23386に準拠する必要があります。|有効なURIの1つのソースは[bSDD](https://search.bsdd.buildingsmart.org/)です。"Fire Rating"のURIの例：[https://identifier.buildingsmart.org/uri/buildingsmart/ifc/4.3/prop/FireRating](https://identifier.buildingsmart.org/uri/buildingsmart/ifc/4.3/prop/FireRating)。|

## 例

|適用対象の意図|要求条件の意図|ファセット定義|
|---|---|---|
|音響等級を持つ任意の壁エンティティ|エンティティ（例：壁）は音響等級を持つ必要がある|Property Set="Pset_WallCommon", Name="AcousticRating"|
|"2HR"の耐火等級を持つ任意の柱エンティティ|エンティティ（例：柱）は"2HR"の耐火等級を持つ必要がある|Property Set="Pset_ColumnCommon", Name="FireRating", value="2HR"|
|20-100立方メートルの間の正味体積を持つ任意のスラブエンティティ|エンティティ（例：スラブ）は20-100立方メートルの間の正味体積を持つ必要がある|Property Set="Qto_SlabBaseQuantities", Name="NetVolume", Value="[20<=Value<=100](https://claude.ai/chat/restrictions.md)"|
|任意の現場打ちまたはプレキャストコンクリート要素|エンティティ（例：スラブ）は、現場打ちまたはプレキャストのいずれかに設定された施工方法を持つ必要がある|Property Set="Pset_ConcreteElementGeneral", Name="CastingMethod", value=["INSITU", "PRECAST"]|
|MyCompany_Concreteプロパティセットに格納された、A、B、またはCから選択されるConcreteMixという名前のカスタムプロパティを持つ任意のエンティティ|エンティティは、MyCompany_Concreteという名前のプロパティセットに格納された、A、B、またはCから選択される値を持つConcreteMixという名前のカスタムプロパティを持つ必要がある|Property Set="MyCompany_Concrete", Name="ConcreteMix", value=["A", "B", "C"]|