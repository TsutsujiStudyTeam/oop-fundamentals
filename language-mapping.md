# 言語別対応表 — 本には書いてないオブジェクト指向

原典は Java を前提に書かれているが、原則は言語に依存しない。設計・実装・レビューの際は、原典の概念を利用者の言語に次のように読み替える。表にない言語は、最も近い列を参考に同じ基準で判断する。

## 概念の対応
| 原典の概念 | Java | C# | TypeScript | Python | Kotlin | Go |
|---|---|---|---|---|---|---|
| クラス（データ構造+処理） | class | class / record + メソッド | class | class / `@dataclass` + メソッド | class / data class + メソッド | struct + メソッド |
| 役割（Interface） | interface | interface | interface（構造的型付け） | Protocol / ABC | interface | interface（構造的型付け） |
| 実現 | implements | : I | implements（または構造的に満たす） | 継承 or 構造的 | : I | メソッド集合を満たす |
| 汎化（データ構造を拡張する継承） | extends | : Base | extends | class Sub(Base) | open class / : Base | 埋め込み（embedding） |
| 関数（禁止対象） | static メソッド、フィールドに触れないインスタンスメソッド | static、拡張メソッド | モジュール関数、static | モジュール関数、`@staticmethod` | トップレベル関数、companion object、拡張関数 | パッケージ関数、レシーバを使わないメソッド |
| ユーティリティクラス（禁止対象） | XxxUtil、XxxHelper | static class | `utils.ts`、名前空間 | `utils.py`、`helpers.py` | object XxxUtils | `util` / `common` パッケージ |
| private メソッド（禁止対象） | private | private | private / `#` | `_name`、`__name` | private | 小文字始まりメソッド（非公開） |
| 隠蔽するフィールド | private フィールド + 公開メソッド | private / プロパティ | private / readonly | `_attr` + メソッド | private val + メソッド | 小文字フィールド + メソッド |
| 一覧クラスの内部コレクション | List / Map / TreeMap | List<T> / Dictionary / SortedDictionary | Array / Map | list / dict / sorted | List / Map / sortedMapOf | slice / map + sort |
| 「次の〜を返す()」（Iterator） | Iterator / Iterable | IEnumerable | Iterable / Symbol.iterator | `__iter__` | Iterable | range 関数 / チャネル |
| 「ループは一覧クラスの中」の対象 | for、stream | for、foreach、LINQ | for、map/filter/reduce | for、内包表記、sorted、sum/any/all | for、コレクション関数 | for range |
| 十進数値（小粒クラスの内部表現） | BigDecimal | decimal | Decimal ライブラリ（big.js 等） | decimal.Decimal | BigDecimal | big.Rat / shopspring/decimal |
| 日付（小粒クラスの内部表現） | Calendar / LocalDate | DateOnly / DateTime | Date / Temporal | datetime.date | LocalDate | time.Time |
| 状態区分（伝票入荷状態など） | enum + メソッド | enum + 拡張 or クラス | union 型 + クラス | Enum + メソッド | enum class + メソッド | 型付き定数 + メソッド |
| static な一覧（消費税率一覧） | static クラス／シングルトン | static class | モジュールスコープのインスタンス | モジュールレベルのインスタンス | object | パッケージ変数 |
| メイン関数 | `public static void main` | `Main` | エントリスクリプト | `if __name__ == "__main__"` | `fun main` | `func main` |
| 層の起点（電文受信） | Servlet.doPost | Controller アクション、Minimal API | Express/Next ハンドラ | Flask/FastAPI/Django ビュー | Ktor/Spring ハンドラ | http.HandlerFunc |
| 別メモリ空間への受け渡し | JDBC、O/R マッパ | ADO.NET、EF Core | Prisma、TypeORM、fetch | SQLAlchemy、psycopg、requests | Exposed、JDBC | database/sql、GORM |
| Comparator（例外的関数） | Comparator | IComparer | compareFn | key 関数 | Comparator | sort.Slice の less |
| ファクトリ（関数禁止の対象外） | static of() | static Create() | static of() | `@classmethod` で `cls(...)` を返す | companion object の of() | NewXxx() |

**重要**: 十進数型・日付型・Enum は小粒クラスの**内部表現**であって代替ではない。日付クラスが `datetime.date` や `LocalDate` を内包し、書式化・日付演算・経過日数のメソッドを持つ。標準型を直接属性にすると、書式や演算が利用側に散る。

## 言語別の注意点
- **構造的型付けの言語（TypeScript、Go）**: 明示的な `implements` がなくても、実体クラスを並べて共通シグネチャを抽出し Interface 名を付ける手順は同じ。Interface を先に切らない。
- **関数が第一級の言語（Python、TypeScript、Kotlin、Go）**: モジュール関数は手軽に書けるため「関数禁止」が最も破られやすい。モジュール関数を書く前に「その引数をひとかたまりにしたクラスは何か」を必ず問う。許されるのは (1) エントリポイント (2) 外部システムとのデータ受け渡し (3) 比較関数、および小粒クラスからのみ呼ぶ型レベル処理。
- **record / data class / dataclass / struct**: データ構造だけを持ちメソッドを持たない型は「構造体」であって、著者の言うクラスではない。処理を外の関数に置くと手続き型に戻る。メソッドをその型に持たせる。ただし層の境界で使う電文（DTO、行データ）としての構造体は可。
- **Python**: 小粒クラスは `@dataclass(frozen=True)` + メソッドが標準形。`@staticmethod` と、`cls` を使わない `@classmethod` は関数。`cls(...)` を返すファクトリ `@classmethod` は対象外。`@property` は公開メソッドの一形態として可。内包表記・`sorted`・`sum`・`any`・`all` も一覧クラスの内側に置く。
- **TypeScript**: `type` だけのオブジェクトと分離した関数群は手続き型。class を使い、メソッドを型に持たせる。`utils.ts` が肥大化していたら小粒クラス・一覧クラスの見逃し。`Array.prototype.map/filter/reduce` も一覧クラスの中で使う。不変オブジェクトの `readonly` 公開フィールドはアクセサの省略形として可。可変フィールドは `private` にする。金額の十進数型は decimal.js か big.js（プロジェクトで統一。`number` は使わない）。
- **Go**: struct にメソッドを持たせる形が「データ構造と処理の一体化」。埋め込みはデータ構造の拡張にだけ使う。レシーバのフィールドを使わないメソッドは関数と同じ。`[]T` を関数間で受け渡すのは一覧クラス（struct）の見逃し。
- **C#**: 拡張メソッドは対象型のフィールドに触れない限り関数。LINQ を一覧クラスの外で多用すると、一覧への処理が散る。一覧クラスのメソッド内に閉じ込める。
- **Kotlin**: トップレベル関数・拡張関数・companion object の関数は関数。data class はメソッドを持たせて初めてクラス。

## 実装の骨格（言語非依存の疑似コード）
```text
// 小粒クラス: データ構造（姓, 名）+ 処理。業務クラスはこれを属性に持ち委譲する
class 氏名 { 姓; 名; 氏名を返す() }
class 日付 { 内部: 標準日付型; 書式文字列を返す(); より前か(日付); 経過日数を返す(基準日) -> 日数 }
class 日数 { 値; 単位付きで返す() }         // 演算結果の単位も小粒クラス

// 一件クラスと一覧クラスの対。コレクションは一覧の内側だけ
class 入荷明細 { 商品; 個数; 入荷日; 入荷伝票を返す() }     // 下位 → 上位を返す
class 入荷明細一覧 {
  内部コレクション: [入荷明細]
  反復（次の明細を返す）
  指定日付の明細を取得する(日付)      // 外部ストレージ呼び出しはこの内側
  数量の一番多い明細を返す()           // ループはここ
}

// ヘッダは明細一覧クラスのインスタンスを属性に持つ。集計は最上位に置き、下位へ委譲
class 受注伝票 { 顧客; 受注日; 明細一覧: 受注明細一覧; 受注総額を返す() { return 明細一覧.明細総額を返す(受注日) } }

// 並列な2クラスにまたがる処理は、両方のデータを持つ第3のクラスへ
class 入荷在庫 { 入荷伝票; 在庫商品一覧; 在庫数を増やす() }

// 層の起点は関数。最低限にして即委譲
handler(request) { プログラム = 入荷登録プログラム(request を電文→オブジェクト); プログラム.登録する() }
```

## 実装例: Python
```python
from dataclasses import dataclass, field
from datetime import date
from decimal import Decimal

@dataclass(frozen=True)
class Quantity:                      # 小粒クラス: 数値 + 単位
    value: int
    unit: str = "個"
    def with_unit(self) -> str: return f"{self.value}{self.unit}"

@dataclass(frozen=True)
class Days:                          # 演算結果の単位も小粒クラス
    value: int
    def with_unit(self) -> str: return f"{self.value}日"

@dataclass(frozen=True)
class BusinessDate:                  # 標準 date は内部表現
    value: date
    def formatted(self) -> str: return self.value.strftime("%Y年%m月%d日")
    def is_before(self, other: "BusinessDate") -> bool: return self.value < other.value
    def days_until(self, base: "BusinessDate") -> Days: return Days((base.value - self.value).days)

class LendingLine:                   # 結果クラス（明細一件）。上位を返す
    def __init__(self, slip: "LendingSlip", item, qty: Quantity):
        self._slip, self._item, self._qty, self._returned_on = slip, item, qty, None
    def slip(self) -> "LendingSlip": return self._slip
    def is_overdue(self, base: BusinessDate) -> bool:
        return self._returned_on is None and self._slip.is_past_due(base)
    def overdue_days(self, base: BusinessDate) -> Days: return self._slip.due().days_until(base)

class LendingLines:                  # 一覧クラス。コレクションとループはここだけ
    def __init__(self, lines: list[LendingLine] | None = None): self._lines = list(lines or [])
    def __iter__(self): return iter(self._lines)
    def add(self, line: LendingLine) -> None: self._lines.append(line)
    def overdue(self, base: BusinessDate) -> "LendingLines":
        return LendingLines([l for l in self._lines if l.is_overdue(base)])

class LendingSlip:                   # ヘッダ。明細一覧インスタンスを属性に持つ
    def __init__(self, employee, lent_on: BusinessDate, due: BusinessDate):
        self._employee, self._lent_on, self._due = employee, lent_on, due
        self._lines = LendingLines()
    def due(self) -> BusinessDate: return self._due
    def is_past_due(self, base: BusinessDate) -> bool: return self._due.is_before(base)
    def lines(self) -> LendingLines: return self._lines
    def save(self, storage: "LendingSlipStorage") -> None: storage.insert(self)   # 外部受け渡しは内側から

def main() -> None:                  # 層の起点 = 関数（例外①）。即委譲
    OverdueLinesProgram().run(BusinessDate(date.today()))
```

## 実装例: TypeScript
```typescript
class PersonName {                   // 小粒クラス
  constructor(private readonly sei: string, private readonly mei: string) {}
  full(): string { return `${this.sei} ${this.mei}`; }
}

class OrderLine {                    // 明細一件。上位を返す
  constructor(private readonly order: Order, readonly product: Product, readonly qty: Quantity) {}
  orderOf(): Order { return this.order; }
  amount(on: BusinessDate): Money { return this.product.unitPrice(on).times(this.qty); }
}

class OrderLines implements Iterable<OrderLine> {   // 一覧クラス。配列と map/filter/reduce はここだけ
  private readonly lines: OrderLine[] = [];
  [Symbol.iterator]() { return this.lines[Symbol.iterator](); }
  add(line: OrderLine) { this.lines.push(line); }
  total(on: BusinessDate): Money { return this.lines.reduce((sum, l) => sum.plus(l.amount(on)), Money.zero()); }
}

class Order {                        // ヘッダ。集計は最上位に置き一覧へ委譲
  private readonly lines = new OrderLines();
  constructor(readonly customer: Customer, readonly orderedOn: BusinessDate) {}
  total(): Money { return this.lines.total(this.orderedOn); }
}

interface OrderLinesLayout { render(lines: OrderLines): void }   // 役割: 画面と帳票が実現
```

## レビュー時の言語別「兆候」
| 言語 | 見たら疑う |
|---|---|
| Java / Kotlin / C# | `XxxUtils`、`XxxHelper`、`static` だらけのクラス、拡張関数の山、`private` メソッド多数、`List<Xxx>` を返す service メソッド、stream/LINQ が service 層に散在 |
| TypeScript | `utils/`、`helpers/` ディレクトリの肥大、`type` + 関数群の組合せ、`Array<Xxx>` の持ち回し、`map/filter` がコンポーネントや API 層に散在 |
| Python | `utils.py`、`helpers.py`、`@staticmethod` 多数、dict/list を引数に取る関数群、`_private` メソッド多数、内包表記がビュー層に散在 |
| Go | `util` / `common` パッケージ、レシーバを使わないメソッド、`[]Xxx` や `map[string]any` を関数間で受け渡し |
