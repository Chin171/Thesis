Reference:
1. Rodin: an open toolset for modelling and reasoning in Event-B: https://eprints.soton.ac.uk/271058/1/main.pdf
2.Event-B patterns and their tool support: https://www.research-collection.ethz.ch/server/api/core/bitstreams/e8cd8212-d14f-43f0-a42f-05d2b3b41df6/content
3. Decomposition Structures for Event-B: https://link.springer.com/chapter/10.1007/978-3-642-00255-7_2 
Khái niệm cơ bản về Event-B
# Báo cáo: **Event-B — Khái niệm, lý thuyết và cơ chế hoạt động**

*Phong cách: khoa học, chi tiết. Độ dài: ~3 trang A4.*

---

## 1. Giới thiệu chung

**Event-B** là một phương pháp hình thức (formal method) dùng để mô hình hóa, phân tích và chứng minh tính đúng đắn của hệ thống phần mềm/hệ thống nhúng thông qua ngôn ngữ toán học dựa trên lý thuyết tập hợp và logic bậc nhất. Mục tiêu chính của Event-B là phát hiện lỗi logic ngay ở giai đoạn thiết kế và đảm bảo các tính chất an toàn (safety) hoặc bất biến (invariant) bằng chứng minh toán học. Event-B hỗ trợ phát triển theo từng bước (stepwise refinement), từ mô hình trừu tượng đến mô hình dần hoàn thiện, kèm theo nghĩa vụ chứng minh (proof obligations) tại mỗi bước.

---

## 2. Cấu trúc mô hình và ký hiệu cơ bản

Một mô hình Event-B được cấu thành bởi hai cấu phần chính:

* **Context**: mô tả phần tĩnh (static) của mô hình, bao gồm các tập hợp trừu tượng (SETS), hằng số (CONSTANTS) và tiên đề (AXIOMS). Context không thay đổi trong thời gian hoạt động hệ thống.
* **Machine**: mô tả phần động (dynamic) — trạng thái và hành vi của hệ thống. Machine bao gồm biến trạng thái (VARIABLES), bất biến (INVARIANTS), và các sự kiện (EVENTS).

### 2.1 Ký hiệu cơ bản

* `x ∈ S` : biến `x` thuộc tập `S`.
* `f ∈ A ↔ B` : `f` là ánh xạ (relation) giữa `A` và `B`.
* `x ≔ expr` : phép gán (sau sự kiện) biến `x` nhận giá trị `expr`.
* `∧`, `∨`, `⇒`, `¬` : các toán tử logic thông thường.
* `card(X)` : số phần tử của tập `X`.

### 2.2 Bất biến (Invariant)

Invariant là các điều kiện phải luôn đúng cho mọi trạng thái hợp lệ của Machine. Ví dụ: `inv1: balance ≥ 0` (số dư không âm).

---

## 3. Sự kiện (Events) và hành vi hệ thống

Trong Event-B, **hành vi** được mô tả bằng tập các **sự kiện**. Mỗi sự kiện có thể có:

* **THEN/ACT**: các hành động gán cập nhật biến.
* **WHERE/GRD**: các điều kiện tiền đề (guards) để sự kiện có thể xảy ra.
* **ANY**: khai báo tham số cục bộ cho sự kiện.

Mỗi sự kiện phải **bảo toàn bất biến**: tức là nếu bất biến đúng trước khi sự kiện xảy ra và điều kiện tiền đề thỏa, thì sau khi sự kiện hành động xong, bất biến vẫn phải đúng.

---

## 4. Refinement (tinh tiến mô hình)

Refinement là cơ chế chính giúp phát triển hệ thống một cách có kiểm soát:

* **Mô hình trừu tượng** (abstract model): mô tả hành vi tổng quát, ít hoặc không quan tâm đến chi tiết triển khai.
* **Mô hình tinh (refined model)**: thêm các biến, sự kiện cụ thể hơn hoặc tinh chỉnh sự kiện trừu tượng thành một hoặc nhiều sự kiện thực thi cụ thể.

Mỗi bước refinement phát sinh thêm nghĩa vụ chứng minh để đảm bảo tính *behavioral consistency* giữa mô hình tinh và mô hình trừu tượng (gọi là **simulation correctness** hoặc **refinement proof obligations**). Các ràng buộc cần chứng minh thường bao gồm: tồn tại mối liên hệ (gluing invariant), bảo toàn bất biến, và tính khả thi của hành vi.

---

## 5. Proof obligations (Nghĩa vụ chứng minh)

Event-B tự động sinh nhiều loại Proof Obligations (POs), các PO chính là:
PO (Proof Obligation) là viết tắt của nghĩa vụ chứng minh — tức là một mệnh đề logic mà mô hình Event-B phải chứng minh là đúng để đảm bảo hệ thống an toàn, đúng đắn và không vi phạm logic.
Khi bạn viết mô hình trong Event-B (gồm variables, invariants, events, refinements…), công cụ Rodin sẽ tự động sinh ra một tập hợp các PO tương ứng.
Mỗi PO đại diện cho một “cam kết” mà mô hình phải thỏa mãn — ví dụ: “bất biến phải luôn đúng sau mọi sự kiện”.

1. **Invariant preservation PO**: với mọi sự kiện `E`, từ invariant và tiền đề (guards) suy ra invariant vẫn đúng sau khi thực hiện action.
2. **Feasibility PO**: hành động của sự kiện không tạo ra phép gán vô nghĩa; tức tồn tại các giá trị biến thỏa điều kiện.
3. **Refinement PO**: mô hình refined thực hiện đúng (simulates) mô hình abstract theo `gluing invariant`.
4. **Well-definedness PO**: biểu thức toán học dùng trong mô hình phải có nghĩa (ví dụ không chia cho 0, hàm hợp lệ...).
5. **Initialisation Correctness**: Trạng thái khởi tạo (initialisation) làm cho mọi invariant đúng

Nếu tất cả các PO đều được chứng minh đúng (✓), mô hình Event-B của bạn được coi là hợp lệ (consistent model).
Ngược lại, nếu còn PO chưa được chứng minh (×), mô hình vẫn có thể chứa lỗi logic.
Các PO được chứng minh bằng công cụ (auto-provers, SMT solvers) hoặc chứng minh thủ công nếu tự động không giải được.

---

## 6. Công cụ hỗ trợ

* **Rodin Platform**: IDE chính thức cho Event-B (plugin trong Eclipse). Rodin hỗ trợ soạn thảo model, sinh PO tự động, và tích hợp trình chứng minh tự động/thuần tay.
* **ProB**: công cụ mô phỏng (animation), model checking, phát hiện deadlock và kiểm tra invariant thông qua duyệt không gian trạng thái. ProB thường được tích hợp như một plugin trong Rodin.
* Ngoài ra có các plugin như **iUML-B** (chuyển đổi UML sang Event-B), **EventB2Java** (sinh mã) để hỗ trợ thiết kế-đến-triển khai.

---

## 7. Ví dụ minh họa (mô hình đơn giản)

Dưới đây là ví dụ ngắn về một hệ thống quản lý tập `USERS` với giới hạn `maxUsers`. Mục đích: mô tả Context, Machine, một event `add_user` và một event `remove_user`.

### 7.1 Context

```event-b
CONTEXT C1
SETS USER
CONSTANTS maxUsers
AXIOMS
  axm1: maxUsers ∈ ℕ
  axm2: maxUsers > 0
END
```

### 7.2 Machine

```event-b
MACHINE M1
SEES C1
VARIABLES users
INVARIANTS
  inv1: users ⊆ USER
  inv2: card(users) ≤ maxUsers
INITIALISATION
  act0: users ≔ ∅
EVENTS
  EVENT add_user
    ANY u
    WHERE
      grd1: u ∈ USER
      grd2: u ∉ users
      grd3: card(users) < maxUsers
    THEN
      act1: users ≔ users ∪ {u}
  END

  EVENT remove_user
    ANY u
    WHERE
      grd1: u ∈ users
    THEN
      act1: users ≔ users \ {u}
  END
END
```

**Phân tích PO**: Rodin sẽ sinh PO để chứng minh rằng sau `add_user` hoặc `remove_user`, `inv1` và `inv2` vẫn giữ đúng. Ví dụ, với `add_user`, cần chứng minh `card(users ∪ {u}) ≤ maxUsers` khi `card(users) < maxUsers` — điều này đúng nhờ bất biến và guard.
Dưới đây là phiên bản **Markdown** của đoạn bạn đưa, được format rõ ràng, có header, code block và bảng để dễ đọc:

# Phân tích Proof Obligations (PO) – Hệ thống USERS

## 1. Tóm tắt mô hình

### Context C1
- **USER**: tập hợp người dùng trừu tượng (một *carrier set*).  
- **maxUsers**: hằng số, số lượng người dùng tối đa.  
- **Axioms**:
  - `maxUsers ∈ ℕ`
  - `maxUsers > 0`  

> Đây là phần “nền tảng toán học” cho Machine, đảm bảo rằng `maxUsers` có nghĩa.

### Machine M1
- **Biến trạng thái**: `users` (tập con của `USER`).  
- **Bất biến (invariants)**:
  - `inv1: users ⊆ USER`
  - `inv2: card(users) ≤ maxUsers`  
- **Khởi tạo**: `users ≔ ∅`  
- **Sự kiện**: `add_user`, `remove_user`  

---

## 2. Các loại Proof Obligations sinh ra

Rodin tự động sinh các loại PO sau:

| Loại PO | Ý nghĩa |
|---------|---------|
| WD (Well-Definedness) | Đảm bảo các biểu thức có nghĩa toán học |
| INV (Invariant Preservation) | Kiểm chứng các invariant luôn đúng sau mỗi event |
| FIS (Feasibility) | Chứng minh hành động trong event có thể thực hiện được |
| INIT (Initialisation) | Chứng minh trạng thái khởi tạo thỏa mãn các invariant |

---

## 3. Phân tích PO chi tiết

### (A) Initialisation PO
```text
act0: users ≔ ∅



**Cần chứng minh**: Sau khi khởi tạo, các invariant `inv1` và `inv2` đều đúng.

| PO              | Mệnh đề cần chứng minh | Giải thích                                      |
| --------------- | ---------------------- | ----------------------------------------------- |
| `INIT/inv1/INV` | ∅ ⊆ USER               | ∅ luôn là tập con của mọi tập                   |
| `INIT/inv2/INV` | card(∅) ≤ maxUsers     | 0 ≤ maxUsers, đúng vì `maxUsers > 0` theo axiom |

> Cả hai PO đều chứng minh được → trạng thái khởi tạo hợp lệ.

---

### (B) Event `add_user`

```event-b
EVENT add_user
  ANY u
  WHERE
    grd1: u ∈ USER
    grd2: u ∉ users
    grd3: card(users) < maxUsers
  THEN
    act1: users ≔ users ∪ {u}
END
```

#### (1) Well-Definedness PO

* Biểu thức `users ∪ {u}` hợp lệ vì cả `users` và `{u}` là tập.
* `card(users)` có nghĩa vì `users` là tập hữu hạn (theo invariant).
  => WD PO hợp lệ.

#### (2) Feasibility PO

* Cần tồn tại `u` thỏa mãn guard:

```
∃ u · (u ∈ USER ∧ u ∉ users ∧ card(users) < maxUsers)
```

→ Nếu `USER` không rỗng và `card(users) < maxUsers`, luôn tồn tại ít nhất một `u` mới chưa trong `users`.
=> PO khả thi.

#### (3) Invariant Preservation POs

Giả sử:

* `users ⊆ USER`
* `card(users) ≤ maxUsers`
* `u ∈ USER, u ∉ users, card(users) < maxUsers`

Cần chứng minh sau `users ≔ users ∪ {u}`:

| PO                  | Mệnh đề cần chứng minh                                                | Giải thích                                                       |
| ------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------- |
| `add_user/inv1/INV` | (users ⊆ USER ∧ u ∈ USER) ⇒ (users ∪ {u} ⊆ USER)                      | Hợp của hai tập con của USER vẫn thuộc USER                      |
| `add_user/inv2/INV` | (card(users) < maxUsers ∧ u ∉ users) ⇒ (card(users ∪ {u}) ≤ maxUsers) | Khi thêm 1 phần tử mới, kích thước tăng 1 đơn vị, vẫn ≤ maxUsers |

> Cả hai đều được chứng minh hợp lệ → event bảo toàn invariant.

---

### (C) Event `remove_user`

```event-b
EVENT remove_user
  ANY u
  WHERE
    grd1: u ∈ users
  THEN
    act1: users ≔ users \ {u}
END
```

#### (1) Well-Definedness PO

* Biểu thức `users \ {u}` hợp lệ vì `users` là tập.
  => WD hợp lệ.

#### (2) Feasibility PO

* Cần tồn tại `u ∈ users`.
* Nếu `users ≠ ∅`, điều này đúng.
  => PO khả thi.

#### (3) Invariant Preservation POs

Giả sử:

* `users ⊆ USER`
* `card(users) ≤ maxUsers`
* `u ∈ users`

Cần chứng minh sau `users ≔ users \ {u}`:

| PO                     | Mệnh đề cần chứng minh                                                | Giải thích                                          |
| ---------------------- | --------------------------------------------------------------------- | --------------------------------------------------- |
| `remove_user/inv1/INV` | (users ⊆ USER) ⇒ (users \ {u} ⊆ USER)                                 | Khi bỏ 1 phần tử, tập vẫn là tập con của USER       |
| `remove_user/inv2/INV` | (card(users) ≤ maxUsers ∧ u ∈ users) ⇒ (card(users \ {u}) ≤ maxUsers) | Khi xóa 1 phần tử, kích thước giảm → vẫn ≤ maxUsers |

> Cả hai PO đều đúng.

---

## 4. Tổng hợp kết quả POs

| Loại PO                      | Initialisation | add_user | remove_user |
| ---------------------------- | -------------- | -------- | ----------- |
| WD (Well-Definedness)        | ✓              | ✓        | ✓           |
| FIS (Feasibility)            | –              | ✓        | ✓           |
| INV (Invariant Preservation) | ✓              | ✓        | ✓           |
| INIT (Initialisation)        | ✓              | –        | –           |

> Tất cả các PO đều chứng minh đúng → mô hình hợp lệ.

---

## 5. Sơ đồ minh họa luồng PO trong Rodin

```
Context (C1) ──► Machine (M1)
                   │
                   ├── Invariants (inv1, inv2)
                   │
                   ├── Initialisation → PO(init/inv1), PO(init/inv2)
                   │
                   ├── add_user → PO(WD), PO(FIS), PO(inv1), PO(inv2)
                   │
                   └── remove_user → PO(WD), PO(FIS), PO(inv1), PO(inv2)
```


