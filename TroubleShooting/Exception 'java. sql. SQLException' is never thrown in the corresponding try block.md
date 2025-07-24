---
sticker: lucide//check-square
tags:
  - error
---
# 문제
- 해당 코드에서 발생
```java
private void close(Connection conn, PreparedStatement pstmt, ResultSet rs) {  
  try {  
    if (rs != null) {  
      rs.close();  
    }  
  } catch (SQLException e) {  
    e.printStackTrace();  
  }  
  
  try {  
    if (pstmt != null) {  
      pstmt.close();  
    }  
  } catch (SQLException e) {  
    e.printStackTrace();  
  }  
  
  try {  
    if (conn != null) {  
      close(conn);  
    }  
  } catch (SQLException e) { // 여기!  
    e.printStackTrace();  
  }  
}  
  
private void close(Connection conn) {  
  DataSourceUtils.releaseConnection(conn, dataSource);  
}
```

# 원인
- 해당 구문에서 SQLException이 던져질 케이스가 없다

# 해결
- catch 문 삭제
- try 문 내에서 해당 exception 명시적으로 던지기

# 참고
- https://stackoverflow.com/questions/22613423/exception-is-never-thrown-in-body-of-corresponding-try-statement