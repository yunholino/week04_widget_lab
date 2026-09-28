import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: '상품 상세 화면',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        useMaterial3: true,
      ),
      home: const ProductDetailPage(),
    );
  }
}

class ProductDetailPage extends StatefulWidget {
  const ProductDetailPage({super.key});

  @override
  State<ProductDetailPage> createState() => _ProductDetailPageState();
}

class _ProductDetailPageState extends State<ProductDetailPage> {
  // 상태 관리 변수
  bool _isLiked = false;
  int _quantity = 1;
  final int _unitPrice = 25000;

  // 리뷰 목록 데이터
  final List<String> _reviews = [
    '배송이 빠르고 착용감이 좋습니다.',
    '실물 색상이 더 예쁘네요.',
    '가성비 대비 만족스럽습니다.',
    '재구매 의사 있습니다!',
  ];

  void _toggleLike() {
    setState(() {
      _isLiked = !_isLiked;
    });
  }

  void _incrementQuantity() {
    setState(() {
      _quantity++;
    });
  }

  void _decrementQuantity() {
    if (_quantity > 1) {
      setState(() {
        _quantity--;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    // 1. Scaffold & AppBar: 기본 화면 틀 구성
    return Scaffold(
      appBar: AppBar(
        title: const Text('상품 상세 정보'),
        centerTitle: true,
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
      ),
      // 2. Column: 화면 전체 요소를 세로로 배치
      body: Column(
        children: [
          // 3. Image: 상품 이미지 표시 (네트워크 이미지)
          Image.network(
            'https://picsum.photos/id/1062/600/300',
            height: 180,
            width: double.infinity,
            fit: BoxFit.cover,
            errorBuilder: (context, error, stackTrace) {
              return Container(
                height: 180,
                color: Colors.grey[300],
                child: const Center(child: Text('이미지를 불러올 수 없습니다.')),
              );
            },
          ),
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                // 4. Row: 상품명과 좋아요 버튼을 가로로 배치
                Row(
                  mainAxisAlignment: MainAxisAlignment.spaceBetween,
                  children: [
                    // 5. Text: 상품명 표시
                    const Text(
                      '스마트 워치 밴드',
                      style: TextStyle(
                        fontSize: 22,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    // 6. Icon & IconButton: 좋아요 버튼 동작
                    IconButton(
                      icon: Icon(
                        _isLiked ? Icons.favorite : Icons.favorite_border,
                        color: _isLiked ? Colors.red : Colors.grey,
                        size: 28,
                      ),
                      onPressed: _toggleLike,
                    ),
                  ],
                ),
                const SizedBox(height: 8),
                // Text: 수량 변경에 따른 실시간 총 가격 표시
                Text(
                  '가격: ${(_unitPrice * _quantity).toString()}원',
                  style: const TextStyle(
                    fontSize: 18,
                    color: Colors.blue,
                    fontWeight: FontWeight.w600,
                  ),
                ),
                const SizedBox(height: 12),
                // Row: 수량 조절 버튼 및 구매 버튼 배치
                Row(
                  children: [
                    const Text('수량: ', style: TextStyle(fontSize: 16)),
                    IconButton(
                      icon: const Icon(Icons.remove_circle_outline),
                      onPressed: _decrementQuantity,
                    ),
                    Text('$_quantity', style: const TextStyle(fontSize: 16)),
                    IconButton(
                      icon: const Icon(Icons.add_circle_outline),
                      onPressed: _incrementQuantity,
                    ),
                    const Spacer(),
                    // ElevatedButton: 클릭 시 실행 결과(SnackBar) 표시
                    ElevatedButton(
                      onPressed: () {
                        ScaffoldMessenger.of(context).showSnackBar(
                          SnackBar(content: Text('총 $_quantity개 구매 완료되었습니다.')),
                        );
                      },
                      child: const Text('구매하기'),
                    ),
                  ],
                ),
              ],
            ),
          ),
          const Divider(),
          const Padding(
            padding: EdgeInsets.symmetric(horizontal: 16.0, vertical: 8.0),
            child: Align(
              alignment: Alignment.centerLeft,
              child: Text(
                '상품 후기',
                style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
              ),
            ),
          ),
          // 7. ListView: 스크롤 가능한 리뷰 목록 배치
          Expanded(
            child: ListView.builder(
              itemCount: _reviews.length,
              itemBuilder: (context, index) {
                return ListTile(
                  leading: const Icon(Icons.rate_review),
                  title: Text(_reviews[index]),
                  subtitle: Text('작성자: user0${index + 1}'),
                );
              },
            ),
          ),
        ],
      ),
    );
  }
}
