# Lớp API — Trung Gian Cho Ứng Dụng Khác

## Vì sao không để mọi ứng dụng tự viết SPARQL thô

Nếu dashboard, agent AI, mobile app... mỗi nơi tự viết SPARQL riêng để gọi thẳng vào triple store, sẽ có vấn đề:
- Logic nghiệp vụ (ví dụ "SKU đủ điều kiện sản xuất là gì") bị lặp lại rải rác ở nhiều nơi
- Khó kiểm soát — một query sai cú pháp hoặc quá nặng (quét toàn bộ graph) có thể làm chậm cả hệ thống
- Agent AI (Tuần 5) tự sinh SPARQL có rủi ro bảo mật/logic sai cao hơn gọi một API đã kiểm soát sẵn

## Viết API bọc lại bằng FastAPI

```python
from fastapi import FastAPI
import requests

app = FastAPI()
FUSEKI = "http://localhost:3030/sanxuat/sparql"

@app.get("/skus-by-style/{style_id}")
def skus_by_style(style_id: str):
    query = f"""
    PREFIX nm: <http://nhamay.vn/ontology/sanxuat#>
    SELECT ?sku ?size WHERE {{
        ?sku nm:belongsToStyle nm:{style_id} ;
             nm:hasSizeLabel ?size .
    }}
    """
    r = requests.get(FUSEKI, params={"query": query},
                      headers={"Accept": "application/sparql-results+json"})
    return r.json()["results"]["bindings"]
```

Chạy thử: `uvicorn main:app --reload`, rồi gọi `http://localhost:8000/skus-by-style/StyleA123`.

## Vai trò lớp này với Tuần 5

Lớp API này chính là cái mà **agent AI** ở Tuần 5 sẽ gọi — thay vì tự sinh SPARQL (dễ sai cú pháp, khó kiểm soát), agent gọi một "tool" đã được định nghĩa rõ ràng, đã test kỹ, an toàn hơn nhiều (đây chính là "Cách B — Function calling" sẽ nói ở Tuần 5).

## Lưu ý bảo mật quan trọng

Đoạn code trên nối `style_id` trực tiếp vào chuỗi SPARQL — **có rủi ro SPARQL injection** y hệt SQL injection, nếu `style_id` đến từ input người dùng không kiểm soát. Ở quy mô học tập, biết trước rủi ro này là đủ; khi lên thực tế, nên dùng tham số hóa (parameterized query, tương tự prepared statement trong SQL) hoặc validate chặt `style_id` chỉ chứa ký tự an toàn (chữ/số) trước khi nối chuỗi.

---
*Tiếp theo: [06 — Bài tập thực hành](06-bai-tap-thuc-hanh.md)*
