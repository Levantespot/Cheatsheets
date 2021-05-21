## Logic:

- `Assertion`: 断言
- `Hypothesis`: 假设
- `True, False`: 真，假
- `A ⇒ B`: “A implies B” A 推出 B, or “From A follows B” 若 A 则 B, or “If A (is true), then (so is) B.” 若 A 为真则 B 为真
- `A⇔B`: “A is equivalent to B” A 等价于 B, or “A holds if, and only if, B holds”. A 成立当且仅当 B 成立
- `Element`: 元素
- `Set`: 集
- `Collection`: 类
- `x∈A`: “x is in A”. x 属于 A
- `A⊆B`: “A is a subset of B”. A 包含于 B
- `∀x`: “For all x” (or “For every x”). 对任意 x
- `∃x`: “For some x”, or (there exists x). 存在 x

## Basic math vocabulary:

- `x≥y`: x is greater than y, or y is less than x. x 不小于 y
- `x≥0`: x is positive, and −x−x is negative. x不小于00
- `x>y` or `x>0`: x is strictly greater than y, or x is strictly positive. x严格大于y x严格大于0
- `N`: natural numbers (自然数) sometimes called strictly positive integers (正整数).
- `Z`: integers 整数
- `Q`: rational numbers 有理数
- `R`: real numbers 实数
- `f:X→Y`: function f from the domain X to the codomain Y. f 是从定义域 X 映射到值域 Y 的函数
- `f(A)` and `f^-1(B)`: image of A⊆X and preimage of B⊆Y under f. A 在映射f下的像，B的原像
- `f(x)≤f(y) for x≤y`: f is increasing. 单调不减的
- `f(x)≥f(y) for x≤y`: f is decreasing. 单调不增的
- `f either increasing or decreasing`: f is monotone. f 是单调的
- `Finite family` 有限族
- `Countable family` 可数族

## Writing proofs:

- We show that A and B implies C. 我们说明A 和 B 推出C
- Under the assumption [of the theorem/proposition/exercise], it holds [this/that].根据定理的假设，我们可以得出
- Suppose/assume that [something] holds.假设…成立
- By contradiction, suppose that [something] holds. (show that it can not be true).由于矛盾，说明…成立
- If [something] holds, then it follows that … 如果…成立则说明…
- Since [something] holds, it follows that… 因为…成立，我们有…
- Hence,… 由于
- [something] yields [something] …表明…
- However, [something is true], therefore, [another thing] holds. …成立，因此，…成立
- Thus,… 那么
- This completes the proof. (or CQFD.) 证明完成

## Exemplary sentences of the lecture:

- Let x∈R be such that… 令x属于实数集则有
- Let (x_n) be a sequence of elements in A such that… 令(x_n)是 A 中的一列元素
- Let f:X→Y be a function such that… 令f是从X映射到Y的函数，那么
- Let (A_i) be a family of subsets of Ω such that… 令(Ai)是(Ω)中的子集族
- Let (A_n) be a countable family of subsets of Ω such that…令(Ai)是(Ω)中的可数子集族
- Since F is a σ-algebra, it follows that ∪A_n∈F for every countable family (An) of elements in F. 因为 F 是一个 σ-代数, 对任意由 F中的元素(An)组成的可数集族可以得到 ∪An∈F
- Let (Ak)k≤n(Ak)k≤n be a finite family of subsets of ΩΩ such that…令(Ak)k≤n(Ak)k≤n是ΩΩ的可数子集族，那么
- Suppose that (An)(An) is a countable family of subsets of ΩΩ such that…假设(An)(An)是ΩΩ的一个可数子集族
- Since the random variable X is measurable, it follows that {X≤a}{X≤a} is measurable for every a∈Ra∈R.由于随机变量X可测，可以得到{X≤a}{X≤a}对任意a∈Ra∈R可测
- Suppose that the random variable X is bounded, then it follows in particular that X is integrable. 假设随机变量X有界，那么特别地，X是可积的

## Example

(推理)
Let nn and mm be two integers.Suppose that nn is even and mm is even. Then, nmnm is also even.

令 nn 和 mm 是两个整数. 假设 nn 是偶数 mm 是偶数. 那么 nmnm 也是偶数.

(not nice but okay at the beginning) 证明（不完美的写法）
n,m∈Zn,m∈Z are even ⇒⇒ ∃p,q∈Z∃p,q∈Z such that n=2pn=2p and m=2qm=2q ⇒nm=2p2q=2(2pq)=2r⇒nm=2p2q=2(2pq)=2r where r:=2pq∈Zr:=2pq∈Z ⇒⇒ nmnm is even. CQFD.

n,m∈Zn,m∈Z 是偶数 ⇒⇒ ∃p,q∈Z∃p,q∈Z 使得 n=2p 且 m=2q ⇒nm=2p2q=2(2pq)=2r其中r=2pq∈Z⇒nm=2p2q=2(2pq)=2r其中r=2pq∈Z ⇒⇒ mnmn 也是偶数. 证明完成.

(证明)
Let nn and mm be two even integers. By definition, it follows that nn and mm are divisible by two, that is, n=2pn=2p and m=2qm=2q for some integers pp and qq. Hence, nm=2p2q=4pq=2(2pq)nm=2p2q=4pq=2(2pq). It follows that nm=2rnm=2r where r=2pqr=2pq is an integer. Thus, nmnm is even, which completes the proof.

令 nn 和 mm 是两个偶数. 根据定义，mm 和 nn 可以被 22 整除，这说明 n=2pn=2p 且 m=2qm=2q 对某些整数 p,qp,q 成立. 那么 mn=2rmn=2r 其中 r=2pqr=2pq 也是整数. 那么 nmnm 是偶数，证明完成.