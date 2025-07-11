# 開発者ガイド

IDSファイルは単にXMLファイルであり、そのスキーマはXSDで定義されています。既存のIDSファイルを開いてその内容を確認することで、IDSがどのように構造化されているかを理解できます。

IDSは、XSDベースの検証チェックを通過すれば有効とみなされます。buildingSMARTのパブリックIDSテンプレートディレクトリで利用可能なすべてのサンプルIDSファイルは、有効であることが保証されています。

1. [最新のIDS XSDスキーマをダウンロード](https://github.com/buildingSMART/IDS/blob/master/Development/ids.xsd)
2. `Documentation/ImplementersDocumentation/TestCases`フォルダからサンプルIDSファイルをダウンロード

XSD検証を実行できる無料のオンラインツールやプログラミングライブラリは多数あります。 しかし、有効なIDSファイルには、単なるXMLスキーマの準拠以上のものが必要です。buildingSMARTは[IDS監査ツール](https://github.com/buildingSMART/IDS-Audit-tool/)を提供し、あなたが作成または受信するIDSファイルが完全に有効であることを確認できます。同じツールは[Xbim IDS監査サービス](https://www.xbim.it/ids)でも利用可能で、これはWebブラウザでローカルに実行され、IDSファイルをどのサーバーにもアップロードしません。

## IDSの作成

IDSファイルの読み込みと作成のみを行うソフトウェアを作成する場合、以下の基準を満たす **必要があります** ：

- すべてのIDSソフトウェアは、有効なIDSファイルのみを読み込み、書き込みする必要があります。不正なIDSファイルを読み込むために何らかの復旧が必要な場合、ユーザーに問題と自動復旧イベントについて通知する必要があります。
- 独自の拡張機能は許可されません。補助システム（例：追加で読み込まれたメタデータ）がIDSまたは関連するIFCモデルを拡張するために使用される場合、それがIDSの外部であることをユーザーに明確にする必要があります。
- データの損失は発生してはいけません。IDSを読み込んで保存することで、すべての情報が保持される必要があります。データが変更されない限り、軽微な構文フォーマットの変更は許可されます。
- スキーマの任意の`xs:sequence`内のxml entities の順序は尊重される必要があります。このxml機能の使用は、ファイル間での内容の比較を簡単にすることを目的としています。

さらに、ユーザーに以下の機能を提供することも強く推奨されます：

- ユーザーが **Entity Facet** でIFCクラスを記述する場合、インターフェースは要件(Specification)の選択されたIFCスキーマの有効なIFCクラス名に許可値を制限する必要があります。オートコンプリートが推奨されます。
- ユーザーが **Entity Facet** で事前定義型を記述する場合、インターフェースは指定されたIFCクラスに基づいて許可値を推奨する必要があります。ただし、ユーザーがカスタム事前定義型を記述することも許可する必要があります。オートコンプリートが推奨されます。
- ユーザーが既に **Entity Facet** を指定し、**Attribute Facet** を作成している場合、インターフェースは指定されたIFCクラスに基づいて許可値を制限する必要があります。インターフェースは、指定された基本属性(Attribute)名に基づいて適切なデータ型を使用するようユーザーを導く必要があります。
- ユーザーが既に **Entity Facet** を指定し、**Property Facet** を作成している場合、インターフェースは指定されたIFCクラスと事前定義型に基づいて許可プロパティセットを推奨する必要があります。ただし、ユーザーがカスタムプロパティセット名を記述することも許可する必要があります。標準化された（例：`Pset_`または`Qto_`）プロパティセットが指定された場合、インターフェースは許可プロパティ名を制限し、使用する適切なデータ型を推奨する必要があります。
- **Property Facet** でカスタムプロパティに文字列が指定される場合、IfcLabelをデフォルトのデータ型とすることが推奨されます
- **Property Facet** でカスタムプロパティに整数が指定される場合、IfcIntegerをデフォルトのデータ型とすることが推奨されます
- **Property Facet** でカスタムプロパティに浮動小数点数が指定される場合、IfcRealをデフォルトのデータ型とすることが推奨されます
- ユーザーが単位付きの値を指定している場合、ユーザーが希望する単位でIDSを記述できるように変換ツールを提供する必要があります
- 一般的に知られているシステムの標準化された分類名、およびスペルミスを防ぐための分類参照を事前に読み込むことも選択できます。この[分類システムのIFCディレクトリ](https://github.com/Moult/ifcclassification)を使用することを選択できます。
- ユーザーが **Material Facet** を指定している場合、インターフェースはIFC推奨材料カテゴリ（'concrete'、'steel'、'aluminium'、'block'、'brick'、'stone'、'wood'、'glass'、'gypsum'、'plastic'、または'earth'の中から一つ）を推奨する必要があります
- 値を指定する際、XML文字列（simpleValueと制限列挙型(Enumeration)）は、[DataType文書](https://claude.ai/chat/DataTypes.md)に示されている[正規表現(Pattern)](https://claude.ai/chat/DataTypes.md#xml-base-types)に準拠する必要があります。

## IFCに対するIDSチェック

IDSチェックを実装するソフトウェアは、`Documentation/ImplementersDocumentation/TestCases`フォルダで利用可能なIFC/IDSペアのテストスイートに準拠する **必要があります** （[テストケース文書](https://claude.ai/chat/TestCases/scripts.md)を参照）。

さらに、ユーザーに以下の機能を提供することも強く推奨されます：

- IDS監査結果をBCF-XML形式で保存するか、BCF-API経由でOpenCDEに接続することが意図されています。ただし、BCFでのこれらの結果のフォーマットと全体的な構造化は現在指定されていません。
- IDS要件(Specification)で指定されたIFCバージョンをソフトウェアが解析できない場合、ユーザーにその制限について認識させる必要があります。
- 要求条件(Requirements)がオプションであるが、代わりに必須であった場合に失敗する場合、チェッカーツールはエラーをログに記録してはいけませんが、補助的な警告や推奨事項を提供することができます

### 精度

浮動小数点値は、`x * (1. - 1.e-6) - 1.e-6`と`x * (1. + 1.e-6) + 1.e-6`の範囲（排他的）の間にある場合、数値`x`と等価とみなされます。

これは、精度を小単位から大単位へスケールできる妥協案および簡素化です。

### 制限

XSDには **Total Digits** と **Fraction Digits** 制限も含まれます。これらは実用性が限られているため、IDSではサポートされません。

### オプション性

要件(Specification)は、モデルでの`適用対象(Applicability)`の要求一致に応じて、**Required**、**Optional**、または **Prohibited** に設定できます。 これは、XSDの`minOccurs`と`maxOccurs`機能を使用して表現されます。以下の状態で表されます：

|オプション性|minOccurs|maxOccurs|
|---|---|---|
|Required|1|unbounded|
|Optional|0|unbounded|
|Prohibited|0|0|

`minOccurs`と`maxOccurs`の他の設定は現在許可されていません。

## 利用可能な開発者ライブラリ

開発を開始するのに役立つように、アプリケーションで使用できる[IDSライブラリディレクトリ](https://technical.buildingsmart.org/resources/software-implementations/)があります。

[ライブラリを提出](https://technical.buildingsmart.org/resources/software-implementations/)することを自由に行ってください（ログインが必要です）。

## さらなる読み物

- [ソフトウェアベンダーディレクトリに実装を追加](https://technical.buildingsmart.org/resources/software-implementations/)
- [改善提案を作成](https://github.com/buildingSMART/IDS/issues)