# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-2A202600756
**Name:** lê Thành Đạt
**Date:** 10/6/2026
---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Agent: Based on my data, the best choice is Laptop at $1200. | 9 | Dung san pham, dung gia, khuyen nghi hop ly |
| Garbage Data (`garbage_data.csv`) | Agent: Based on my data, the best choice is Nuclear Reactor at $999999. | 2 | Sai hoan toan — outlier cuc dai duoc de xuat thay vi san pham thuc te |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Garbage data chua nhieu van de nghiem trong khien Agent cho ket qua sai:

1. **Duplicate IDs**: ID=1 xuat hien 2 lan (Laptop va Banana). Khi Agent doc du lieu, co the lay nham ban ghi, dan den ket qua khong nhat quan va kho debug.

2. **Wrong data types**: Gia cua "Broken Chair" duoc ghi la "ten dollars" thay vi so. Pandas doc toan bo cot `price` thanh kieu string. Khi Agent goi `idxmax()` de tim san pham dat nhat, no so sanh chuoi thay vi so — "999999" > "1200" theo thu tu lexicographic, nhung toan bo logic gia tri so bi sai.

3. **Extreme outlier**: "Nuclear Reactor" co gia $999,999 — du lieu bat thuong khong phan anh hang hoa thuc te. Do kieu du lieu bi pha vo (string), no van duoc chon la "best deal" va Agent tu tin de xuat mot san pham phi thuc te.

4. **Null values**: "Ghost Item" khong co ID va khong co category. No lam nhiem du lieu va co the gay loi khi cac pipeline khac xu ly cot `id`.

5. **Inconsistent casing**: Category duoc viet thuong ("electronics") thay vi Title Case. May man la Agent dung `.str.lower()` nen van match duoc, nhung neu logic thay doi, toan bo ket qua co the tra ve rong.

Ket qua: Agent tra loi voi su tu tin cao nhung hoan toan sai — day la truong hop nguy hiem nhat vi nguoi dung co the tin tuong vao cau tra loi ma khong kiem tra lai.

---

## 3. Ket luan

**Quality Data > Quality Prompt?** Dong y.

Du prompt co chinh xac den dau, neu du lieu dau vao bi nhiem boi outlier, null values, sai kieu du lieu hoac duplicate — Agent van tra loi sai. Trong bai nay, cung mot cau hoi, cung mot logic Agent, nhung clean data cho ket qua chinh xac (Laptop/$1200) trong khi garbage data dan den de xuat phi ly (Nuclear Reactor/$999,999). Chat luong du lieu quyet dinh chat luong dau ra; prompt chi la lop boc ben ngoai.
