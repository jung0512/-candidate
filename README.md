# candidate
지역구 후보
# 후보 조회
```db
<%

	StringBuffer sb = new StringBuffer();
	sb.append("select mem.*, pt.*, substr(mem.M_JUMIN,1,6) as f_jumin, substr(mem.M_JUMIN,7,13) as b_jumin, ")
		.append(" case mem.P_SCHOOL")
		.append("    when '1' then '고졸' ")
		.append("    when '2' then '학사' ")
		.append("    when '3' then '석사' ")
		.append("    when '4' then '박사' ")
		.append("    end p_schooln        ")
		.append("from TBL_MEMBER_202005 mem, TBL_PARTY_202005 pt ")
		.append("where mem.P_CODE = pt.p_code                    ")
		.append("order by mem.p_code                             ");
	
	String sql =sb.toString();

	Connection conn = DBConnect.getConnection();
   	PreparedStatement ps = conn.prepareStatement(sql);
   	ResultSet rs = ps.executeQuery();
   	
%>
```
테이블 두개를 사용하여야 하기 때문에 테이블을 조인하여 준다<br>
후보번호,성명,소속정당,지역구,대표전화을 조회하고 학력은 1일 경우 고졸, 2일 경우 학사,3일 경우 석사, 4일 경우 박사로 표시한다<br>
주민번호는 000000-0000000으로 표시하여 출력한다<br>
```
<table class="tableList">
			<tr>
				<td>후보번호</td>
				<td>성명</td>
				<td>소속정당</td>
				<td>학력</td>
				<td>주민번호</td>
				<td>지역구</td>
				<td>대표전화</td>
			</tr>
			<%
			
			while(rs.next()){
			
			%>
			<tr>
				<td><%=rs.getString("m_no")%></td>
				<td><%=rs.getString("m_name")%></td>
				<td><%=rs.getString("p_name")%></td>
				<td><%=rs.getString("p_schooln")%></td>
				<td><%=rs.getString("f_jumin")%>-<%=rs.getString("b_jumin")%></td>
				<td><%=rs.getString("m_city")%></td>
				<td><%=rs.getString("p_tel1")%>-<%=rs.getString("p_tel2") %>-<%=rs.getString("p_tel3") %></td>
			</tr>
			<%} %>
		</table>
```
sql문을 이용하여 테이블을 출력하여준다<br>
![image](https://user-images.githubusercontent.com/102035198/210191869-c4f970fd-c884-4bc3-a3f7-30e9f42daa78.png)<br>
# 투표하기 
```
<script type="text/javascript">
	function remove() {
		alert("모든 내용을 지우고 다시 입력합니다!");
		document.vData.reset();
		document.vData.jumin.focus();
	}
	
	function checkVal(){
		
		if(!document.vData.jumin.value){
			alert("주민번호가 입력되지 않았습니다!");
			document.vData.jumin.focus();
			return false;
		}else if(!document.vData.name.value){
			alert("성명이 입력되지 않았습니다!");
			document.vData.name.focus();
			return false;			
		}else if(!document.vData.vote_num.value){
			alert("후보번호가 입력되지 않았습니다!");
			document.vData.vote_num.focus();
			return false;
		}else if(!document.vData.time.value){
			alert("투표시간이 입력되지 않았습니다!");
			document.vData.time.focus();
			return false;
		}else if(!document.vData.place.value){
			alert("투표장소가 입력되지 않았습니다!");
			document.vData.place.focus();
			return false;
		}else if(!document.vData.confirm.value){
			alert("유권자확인이 선택되지 않았습니다!");
			return false;
		}
		alert("투표하기 정보가 정상적으로 등록되었습니다!");
	}
</script>
```
입력되지 않은 칸이 있을 시 경고창을 띄우고 포커스를 이동시켜주는 유효성 검사이다.<br>
```
<table class="tableList">
				<tr>
					<td>투표하기</td>
					<td class="align-left"><input type="text" size="26" name="jumin"> 예: 8912121111111</td>
				</tr>
				<tr>
					<td>성명</td>
					<td class="align-left"><input type="text" size="20"  name="name"></td>
				</tr>
				<tr>
					<td>후보번호</td>
					<td class="align-left">
						<select name="vote_num">
							<option value="">직업선택</option>
							<option value="1">[1]김후보</option>
							<option value="2">[2]이후보</option>
							<option value="3">[3]박후보</option>
							<option value="4">[4]조후보</option>
							<option value="5">[5]최후보</option>
						</select>
					</td>
				</tr>
				<tr>
					<td>투표시간</td>
					<td class="align-left"><input type="text" size="20"  name="time"></td>
				</tr>
				<tr>
					<td>투표장소</td>
					<td class="align-left"><input type="text" size="20"  name="place"></td>
				</tr>
				<tr>
					<td>유권자확인</td>
					<td class="align-left">
						<input type="radio" id="confirm_y"  name="confirm" value="Y">
						<label for="confirm_y">확인</label>
						<input type="radio" id="confirm_n"  name="confirm" value="N">
						<label for="confirm_n">미확인</label>
					</td>
				</tr>
				<tr>
					<td colspan="2">
						<input type="submit" value="투표하기">
						<input type="button" value="다시하기" onclick="remove()">
					</td>
				</tr>
			</table>
```
주민번호, 성명, 투표시간, 투표장소는 입력창으로 만들어준다<br>
후보번호와 유권자확인은 각각 셀렉트박스와 라이오버튼으로 만들어준다<br>
![image](https://user-images.githubusercontent.com/102035198/210286882-b3948e6e-660d-46a1-aacf-23c358f009fa.png)
![image](https://user-images.githubusercontent.com/102035198/210286891-d637642b-c7c2-4141-b870-f65a0109ec0d.png)
# 
