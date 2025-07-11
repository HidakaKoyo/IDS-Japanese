# エンティティファセット

IFCモデルのすべてのインスタンスには「IFCクラス」（EXPRESSエンティティとも呼ばれます）があります。たとえば、壁のインスタンスはIfcWallというIFCクラスを持ち、ドアのインスタンスはIfcDoorというIFCクラスを持ちます。個々の建物要素を表さないインスタンスにもクラスがあります。たとえば、プロジェクトはIfcProjectクラス、窓タイプはIfcWindowTypeクラス、コスト項目はIfcCostItemクラスを持ちます。

クラスは単にインスタンスを分類するためだけのものではありません。それらはまた、どのような種類のプロパティや関係を持つことが許可されているかを示します。たとえば、IfcWallクラスのインスタンスは耐火等級プロパティを持つことができますが、IfcGridインスタンスは持つことができません。

異なるIFCスキーマには異なるIFCクラスがあります。より最近のIFCスキーマには、より豊富で多様なIFCクラスが含まれています。以下で比較できます：

- [IFC4X3_ADD2 IFCクラス名一覧](http://ifc43-docs.standards.buildingsmart.org/IFC/RELEASE/IFC4x3/HTML/annex-b1.html)
- [IFC4 IFCクラス名一覧](https://standards.buildingsmart.org/IFC/RELEASE/IFC4/ADD2_TC1/HTML/link/alphabeticalorder-entities.htm)
- [IFC2X3 IFCクラス名一覧](https://standards.buildingsmart.org/IFC/RELEASE/IFC2x3/TC1/HTML/alphabeticalorder_entities.htm)

一部のクラスは、オプションで **定義済みタイプ** を持つこともあります。これは、IFCクラスの **名前** に加えて、さらなるレベルのインスタンス分類です。たとえば、IfcWallのインスタンスはSHEARやPARTITIONINGの **定義済みタイプ** を持つことができます。IFCクラスの **名前** はIFC標準によって指定されますが、**定義済みタイプ** にはユーザーによるカスタム値も含まれる場合があります。

IFCスキーマドキュメントには、標準定義済みタイプの一覧が含まれています。以下は、IFC4X3_ADD2スキーマの有効な **定義済みタイプ** の一覧を見つける方法です。すべてのIFCバージョンで手順は同様です。

1. 指定しているIFCクラスのドキュメントページを参照してください。上記のIFCクラス名一覧からアクセスできます。たとえば、[これはIfcWallドキュメントページです](http://ifc43-docs.standards.buildingsmart.org/IFC/RELEASE/IFC4x3/HTML/lexical/IfcWall.htm)。
2. ドキュメントの **属性** セクションまでスクロールして、**PredefinedType** 属性を見つけてください。
3. **PredefinedType** 属性の隣にある列挙型(Enumeration)リンクをクリックして、有効な値の一覧を表示してください。たとえば、IfcWallの場合、リンクをクリックすると[IfcWallTypeEnumのドキュメント](http://ifc43-docs.standards.buildingsmart.org/IFC/RELEASE/IFC4x3/HTML/lexical/IfcWallTypeEnum.htm)に移動します。
4. 有効な **定義済みタイプ** の一覧が表に表示されます。

標準化された **定義済みタイプ** の一覧から選択することを強く推奨します。ただし、それらがプロジェクトに適用されない場合、任意のカスタム値を指定できます。たとえば、**IfcWall** のカスタム **定義済みタイプ** として「RADIATIONBARRIER」を指定することもできます。

要件を記述する際の最も重要な側面の一つは、適切なIFCクラスに適用されることを確認することです。通常、すべての **要件** は、その **適用対象** セクションで使用される **エンティティファセット** を持ちます。

## パラメータ

| パラメータ       | 必須  | 制約許可 | 許可値                                  | 意味                      |
| ----------- | --- | ---- | ------------------------------------ | ----------------------- |
| **名前**      | ✔️  | ✔️   | IFCスキーマからの有効なIFCクラス                  | IFCクラスは正確に一致する必要がある     |
| **定義済みタイプ** | ❌   | ✔️   | IFCスキーマからの有効な定義済みタイプ、または任意のカスタムテキスト値 | IFC定義済みタイプは正確に一致する必要がある |

## 例

|適用対象の意図|要求条件の意図|ファセット定義|
|---|---|---|
|すべての間仕切り壁|間仕切り壁でなければならない|Name="IFCWALL", PredefinedType="PARTITIONING"|
|すべての床スラブ|床スラブでなければならない|Name="IFCSLAB", PredefinedType="FLOOR"|
|ドアタイプスケジュールに文書化されるようなすべてのドアタイプ|ドアタイプでなければならない|Name="IFCDOORTYPE"|
|すべての建物階|建物階でなければならない|Name="IFCBUILDINGSTOREY"|
|図面、スケジュール、マニュアル、要件書などのすべての関連文書|文書でなければならない|Name="IFCDOCUMENTINFORMATION"|
|温水システム、電気回路などのすべての配管システム|配管システムでなければならない|Name=["IFCDISTRIBUTIONSYSTEM", "IFCDISTRIBUTIONCIRCUIT"]|
|作業分解構造における建設スケジューリングなどのすべての建設タスク|建設タスクでなければならない|Name="IFCTASK", PredefinedType="CONSTRUCTION"|

## IFC2X3の特殊ケース

IFC2X3の一部の発生エンティティは、そのタイプオブジェクトによってさらに指定されます。 例として、エアターミナルの定義があります。これはIFC2X3では、IfcFlowTerminalの発生インスタンスとIfcAirTerminalTypeのタイプインスタンスによってエンコードされます。 エンティティファセットには、タイプエンティティ名をさらに指定するためのパラメータがありません。 この場合、IDSはIFC4で導入された規約に従い、これによりIDSベースのチェックがよりスキーマ非依存になります。 この例では、チェックされるエンティティの **名前** はIfcAirTerminal（タイプなし）である必要があり、指定されたマッピングテーブルによって解決される必要があります。 完全なリストはこの[テーブル](https://claude.ai/chat/Documentation/ImplementersDocumentation/ifc2x3-occurrence-type-mapping-table.md)に記載されています。