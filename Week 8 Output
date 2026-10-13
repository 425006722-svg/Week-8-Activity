String processOrderBatch(List<Map<String, dynamic>> orderList) {
  int successfulOrders = 0;
  int failedOrders = 0;
  String report = '--- Batch Processing Report ---\n\n';

  for (int i = 0; i < orderList.length; i++) {
    var currentOrder = orderList[i];

    try {
      String customerName = currentOrder['customerName'];
      
      int itemQuantity = int.parse(currentOrder['itemQuantity'].toString());
      double itemUnitPrice = double.parse(currentOrder['itemUnitPrice'].toString());
      bool isLoyaltyMember = currentOrder['isLoyaltyMember'] == true;

      if (itemQuantity < 0 || itemUnitPrice < 0.0) {
        throw FormatException('Quantity and unit price cannot be negative.');
      }

      double subtotal = itemQuantity * itemUnitPrice;
      int fullBoxesNeeded = itemQuantity ~/ 3;
      int remainingItems = itemQuantity % 3;

      double discount = 0.0;
      if (isLoyaltyMember && subtotal >= 100.0) {
        discount = subtotal * 0.10; 
      } else if (isLoyaltyMember) {
        discount = subtotal * 0.05; 
      } else {
        discount = 0.0;           
      }

      double finalTotal = subtotal - discount;
      bool qualifiesForFreeShipping = finalTotal >= 100.00;

      report += '''
[Order #${i + 1}] Customer: $customerName
  - Quantity: $itemQuantity @ \$$itemUnitPrice each
  - Subtotal: \$$subtotal | Discount: \$$discount | Final: \$$finalTotal
  - Packaging: $fullBoxesNeeded full box(es) of 3, with $remainingItems leftover item(s).
  - Free Shipping Qualified: $qualifiesForFreeShipping
----------------------------------------
''';
      successfulOrders++;

    } catch (e) {
      failedOrders++;
      report += '''
[Order #${i + 1}] ERROR: Failed to process record -> ${e.toString()}
----------------------------------------
''';
    }
  }

  report += '\nSummary: $successfulOrders successful, $failedOrders error(s) handled.';
  return report;
}

Future<void> main() async {
  print('========================================');
  print('    DART ORDER PROCESSING SYSTEM        ');
  print('========================================\n');

  List<Map<String, dynamic>> testOrders = [
    {
      'customerName': 'Timothy John',
      'itemQuantity': 7,
      'itemUnitPrice': 18.50,
      'isLoyaltyMember': true,
    },
    {
      'customerName': 'Jeremy Baldado',
      'itemQuantity': 2,
      'itemUnitPrice': 35.00,
      'isLoyaltyMember': false,
    },
    {
      'customerName': 'Bad Input Test 1',
      'itemQuantity': 'invalid_qty',
      'itemUnitPrice': 15.00,
      'isLoyaltyMember': true,
    },
    {
      'customerName': 'Bad Input Test 2',
      'itemQuantity': 5,
      'itemUnitPrice': -50.00,    
      'isLoyaltyMember': false,
    },
  ];

  String finalReport = processOrderBatch(testOrders);
  print(finalReport);
}
