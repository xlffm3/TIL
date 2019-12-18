JDBC
----

---

### JDBC(Java Database Connectivity)<br>

-	Java를 이용한 DB 접속과 SQL의 실행, 그리고 실행 결과로 얻어진 데이터의 핸들링을 제공하는 방법과 절차에 관한 규약이다.<br><br>
-	Java 내에서 SQL문을 실행하기 위한 API이다<br><br>
-	SQL과 프로그래밍 언어의 통합 접근 중 한 형태이다.<br><br>
-	Java는 표준 인터페이스인 JDBC API를 제공한다.<br><br>
-	DB 벤더, 또는 기타 써드 파티에서는 JDBC 인터페이스를 구현한 드라이버를 제공한다.<br><br>

### JDBC를 이용한 프로그래밍 방법<br>

-	1. `import java.sql.*;`<br><br>
-	2. 드라이버를 로드한다.<br><br>
-	3. Connection 객체를 생성한다.<br><br>
-	4. Statement 객체를 생성 및 질의를 수행한다.<br><br>
-	5. SQL문에 결과물이 있다면 ResultSet 객체를 생성한다.<br><br>
-	6. 모든 객체를 닫는다.<br><br>

### JDBC 클래스의 생성 관계<br>

![image](https://user-images.githubusercontent.com/56240505/69848520-ab489100-12bd-11ea-8f6e-dbfe83b1158f.png)<br><br>

### 실습 코드<br>

```java
public List<Role> getRoles() {
        List<Role> list = new ArrayList<>();

        try {
            Class.forName("com.mysql.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

        String sql = "SELECT description, role_id FROM role order by role_id desc";
        try (Connection conn = DriverManager.getConnection(dburl, dbUser, dbpasswd);
                PreparedStatement ps = conn.prepareStatement(sql)) {

            try (ResultSet rs = ps.executeQuery()) {

                while (rs.next()) {
                    String description = rs.getString(1);
                    int id = rs.getInt("role_id");
                    Role role = new Role(id, description);
                    list.add(role);
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        } catch (Exception ex) {
            ex.printStackTrace();
        }
        return list;
    }
```

-	reference 사이트에 동영상 강의와 실습 코드가 나와 있으니 직접 해보면서 감을 익힌다.<br><br>
-	간결한 예외 처리를 위해, 자원을 반납시켜 주는 **try-with-resource exception** 을 숙지하자.<br><br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/20653/)

---
