import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MainScreen(), // 메인 화면을 MainScreen으로 설정
    );
  }
}

class MainScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: [
          // 화면의 1/6을 주황색으로 채우고, 그 위에 원하는 위치에 텍스트 추가
          Expanded(
            flex: 1,
            child: Stack(
              children: [
                // 주황색 배경
                Container(
                  color: Colors.orange,
                ),
                // 클릭 가능한 첫 번째 텍스트
                Positioned(
                  left: 80,  // x 좌표
                  top: 30,   // y 좌표
                  child: GestureDetector(
                    onTap: () {
                      // 텍스트를 클릭했을 때 새로운 화면으로 이동하여 달력 띄우기
                      Navigator.push(
                        context,
                        MaterialPageRoute(builder: (context) => CalendarScreen()),
                      );
                    },
                    child: Text(
                      '외출',
                      style: TextStyle(
                        color: Colors.white,
                        fontSize: 20,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                  ),
                ),
                // 클릭 가능한 두 번째 텍스트
                Positioned(
                  left: 400,  // x 좌표
                  top: 30, // y 좌표
                  child: GestureDetector(
                    onTap: () {
                      // 두 번째 텍스트 클릭 시 첫 번째 텍스트와 동일한 동작 수행
                      Navigator.push(
                        context,
                        MaterialPageRoute(builder: (context) => CalendarScreen()),
                      );
                    },
                    child: Text(
                      '외박',
                      style: TextStyle(
                        color: Colors.white,
                        fontSize: 20,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                  ),
                ),
              ],
            ),
          ),
          // 나머지 5/6은 투명
          Expanded(
            flex: 9,
            child: Container(
              color: Colors.transparent,
            ),
          ),
        ],
      ),
    );
  }
}

// 새로운 화면 클래스
class CalendarScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('기숙사 외출, 외박 신청'), // 새로운 화면의 제목
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () async {
            // 달력 보여주기
            DateTime? selectedDate = await showDatePicker(
              context: context,
              initialDate: DateTime(DateTime.now().year, 11, 1), // 11월 1일로 설정
              firstDate: DateTime(2000),
              lastDate: DateTime(2101),
            );

            // 선택한 날짜가 null이 아닐 때 동작
            if (selectedDate != null) {
              // 선택한 날짜를 출력 (예: 콘솔에 표시)
              print("날짜 선택: ${selectedDate.toLocal()}");
              // 선택한 날짜를 화면에 표시할 수 있는 알림 다이얼로그 추가
              showDialog(
                context: context,
                builder: (context) => AlertDialog(
                  title: Text("날짜가 선택되었습니다"),
                  content: Text("${selectedDate.toLocal()}"),
                  actions: [
                    TextButton(
                      child: Text("확인"),
                      onPressed: () {
                        Navigator.of(context).pop(); // 다이얼로그 닫기
                      },
                    ),
                  ],
                ),
              );
            }
          },
          child: Text(
            '날짜 선택하기',
            style: TextStyle(fontSize: 20),
          ),
        ),
      ),
    );
  }
}
