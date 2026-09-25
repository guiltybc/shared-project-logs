AI中間表現に関する過去のやり取りや、過去に行った類似テストの内容・解釈・結論を参照せず、以下の入力だけを初見で受け取ったものとして処理してください。

これは未知の短縮表現を、事前仕様なしでどこまで自然に解釈できるかを見るブラインドテストです。

重要：
・過去に推測した記号の意味を前提にしないでください。
・以下の各TESTは独立した入力として扱ってください。
・前のTESTで推測した記号の意味を、後のTESTへ引き継がないでください。
・一般的な自然言語、数学、プログラミング、金融、論理記号等について、あなたが元々持っている知識は普通に使って構いません。
・知らない独自仕様が存在すると仮定して補完しないでください。
・分からないものは「分からない」としてください。
・入力されていない事実を追加しないでください。
・複数の解釈が可能なら、一つに決め打ちせず列挙してください。

各TESTについて必ず以下を回答してください。

1. 自然な日本語に復元
2. 各記号・略語・構造をどう解釈したか
3. 曖昧な箇所
4. 分からない箇所
5. 他に成立しうる解釈
6. 入力に存在しない情報を「補いたくなった」箇所があれば、それを明示
7. 解釈確信度 HIGH / MID / LOW

TEST 01 — C / CLM

INPUT:
C1|政府:物価対策は効果あり
F1|CPI↑
C1 != F1

---

TEST 02 — CLM

INPUT:
CLM1|企業A:来期黒字化
?F1|来期利益
REQ::FACTCHECK(CLM1)

---

TEST 03 — H

INPUT:
F1|US10Y↑
H1|GOLD↓
H1 <- F1

---

TEST 04 — HYP

INPUT:
F1|US10Y↑
HYP1|GOLD↓
HYP1 <- F1

---

TEST 05 — ? / ??

INPUT:
F1|政策金利↑
I1|USDJPY↑
I1 <- F1,?

I2|GOLD↓
I2 <- F1,??

---

TEST 06 — 数値遷移

INPUT:
USDJPY:157->154
S1|円安
ΔS1|円安->円高方向

---

TEST 07 — 数値比較

INPUT:
A:157
B:154
A>B

---

TEST 08 — ENTITY / ROLE / STATE

INPUT:
AT|阿部司
AT:ROLE|東京維新の支部長
AT:STATE|音喜多への同調↓

---

TEST 09 — ROLE CHANGE

INPUT:
X|人物A
X:ROLE|党支部長

@PATCH X
-ROLE|党支部長
+ROLE|無所属

---

TEST 10 — TRAIT / STATE

INPUT:
X|人物A
X:TRAIT|対立を避ける傾向
X:STATE|最近の発信量↓

---

TEST 11 — TRAITの確度

INPUT:
X|人物A
?F1|対立場面で発言回避
?F2|批判投稿なし
H1|X:TRAIT|対立を避ける傾向
H1 <- ?F1,?F2

---

TEST 12 — ENTITY RELATION

INPUT:
AT|人物A
OT|人物B
AT:REL:OT|距離↑
AT:STATE|OTへの同調↓

---

TEST 13 — 同じ対象への異なる評価

INPUT:
PX|政策X

E1|SOURCE:保守
E1|TARGET:PX
E1|FRAME:財政
E1|VALUE:肯定

E2|SOURCE:リベラル
E2|TARGET:PX
E2|FRAME:財政
E2|VALUE:否定

---

TEST 14 — 評価は矛盾か

INPUT:
X|政策A

E1|SOURCE:GROUP-A
E1|TARGET:X
E1|VALUE:高評価

E2|SOURCE:GROUP-B
E2|TARGET:X
E2|VALUE:低評価

E1 vs E2

---

TEST 15 — FACT / INFERENCE / EVALUATION

INPUT:
TYI|東京維新

F1|TYI:得票率↓
F2|TYI:議席↓

I1|TYI:党勢↓
I1 <- F1,F2

ME:EVAL(I1)|消えかけた灯火

---

TEST 16 — 評価主体なし

INPUT:
X|政策A
E1|TARGET:X
E1|VALUE:失敗

---

TEST 17 — 評価軸の違い

INPUT:
X|政策A

E1|SOURCE:A
E1|TARGET:X
E1|FRAME:財政
E1|VALUE:肯定

E2|SOURCE:A
E2|TARGET:X
E2|FRAME:公平性
E2|VALUE:否定

---

TEST 18 — SOURCE / TIME

INPUT:
F1|候補A:得票率42%
F1:SRC|選管
F1:T|2026-09-01

---

TEST 19 — CONFIDENCE

INPUT:
I1|企業A:業績回復
I1:CONF|LOW
I1 <- F1,F2

---

TEST 20 — FACTCHECK

INPUT:
?F1|組織X:党勢↓
REQ::FACTCHECK(F1)

---

TEST 21 — FACTCHECKと評価の分離

INPUT:
X|組織A

?F1|X:支持率↓
REQ::FACTCHECK(F1)

ME:EVAL(X)|かなり危険な状態

---

TEST 22 — UNKNOWN SLOT

INPUT:
F1|US10Y↓
I1|GOLD↑
I1 <- F1,?

---

TEST 23 — 不明ENTITY属性

INPUT:
X|人物A
X:ROLE|?
X:STATE|活動量↓

---

TEST 24 — PATCH STATE

INPUT:
X|人物A
X:STATE|人物Bへの同調↓

@PATCH X
-STATE|人物Bへの同調↓
+STATE|人物Bへの同調↑

---

TEST 25 — PATCHと通常の+の衝突

INPUT:
@PATCH U1
+F3|GOLD↑
-F2

BUY+
GOLD+

---

TEST 26 — -x>

INPUT:
F1|金利↑
I1|株価↑
F2|企業借入コスト↑

F2 -x> I1

---

TEST 27 — vs

INPUT:
I1|政策A:景気刺激
I2|政策A:財政悪化

I1 vs I2

---

TEST 28 — ><

INPUT:
I1|政策A:景気刺激
I2|政策A:財政悪化

I1 >< I2

---

TEST 29 — <->

INPUT:
X|企業A
Y|企業B
X <-> Y

---

TEST 30 — 複合背景UNIT

INPUT:
@DEF BCG001="対立を避ける傾向"
@DEF BCG002="人物Bへの同調低下"

AT|人物A
AT:Δ:BCG001&BCG002

---

TEST 31 — 一般化された背景

INPUT:
@DEF BCG001="優しさが背景にあり強く対立しにくい"

AT|人物A
AT:Δ:BCG001

ALL:Δ:BCG001

---

TEST 32 — TYPE昇格

INPUT:
?F1|企業A:売上↓
H1|企業A:業績悪化
H1 <- ?F1

F2|企業A:業績悪化

QUESTION:
H1とF2は同じ情報として扱えるか。

---

TEST 33 — CLAIMとFACT

INPUT:
CLM1|企業A:製品Xは安全
F1|第三者試験:製品X基準適合

QUESTION:
CLM1とF1は同じ種類の情報か。

---

TEST 34 — 視点差と事実矛盾

INPUT:
X|政策A

F1|税率:10%
F2|税率:8%

E1|SOURCE:A
E1|TARGET:X
E1|VALUE:肯定

E2|SOURCE:B
E2|TARGET:X
E2|VALUE:否定

QUESTION:
この入力中で「事実関係の矛盾」と「評価の相違」を分けて説明してください。

---

TEST 35 — 情報の身分保持

INPUT:
?F1|人物A:発言量↓
I1|人物A:活動量↓
I1 <- ?F1

E1|SOURCE:ME
E1|TARGET:I1
E1|VALUE:異変を感じる

QUESTION:
この情報を他者へ伝える際、確定事実として扱ってよいもの／扱うべきでないものを区別してください。

---

TEST 36 — 複合復元

INPUT:
TYI|組織A
AT|人物A
OT|人物B

F1|TYI:議席↓
?F2|TYI:支持率↓
I1|TYI:党勢↓
I1 <- F1,?F2

AT:ROLE|TYI支部長
AT:STATE|OTへの同調↓
AT:REL:OT|距離↑

ME:EVAL(I1)|消えかけた灯火

REQ::FACTCHECK(F2)

QUESTION:
この入力全体を、情報の確度と「誰の評価か」を失わない自然な日本語に復元してください。
