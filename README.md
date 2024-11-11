<!DOCTYPE html>
<html ng-app="currencyApp">
<head>
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
</head>
<body ng-controller="CurrencyController">
  <h1>Currency Converter</h1>
  <label for="amount">Amount:</label>
  <input type="number" id="amount" ng-model="amount" placeholder="Enter amount" />
  <br><br>
  <label for="fromCurrency">From:</label>
  <select id="fromCurrency" ng-model="fromCurrency" ng-options="currency for currency in currencies"></select>
  <br><br>
  <label for="toCurrency">To:</label>
  <select id="toCurrency" ng-model="toCurrency" ng-options="currency for currency in currencies"></select>
  <br><br>
  <button ng-click="convert()">Convert</button>
  <h2>Converted Amount: {{ convertedAmount | number: 2 }}</h2>

  <script>
    angular.module('currencyApp', [])
      .controller('CurrencyController', ['$scope', '$http', function($scope, $http) {
        $scope.currencies = ['USD', 'EUR', 'GBP', 'INR', 'JPY'];
        $scope.amount = 0;
        $scope.fromCurrency = 'USD';
        $scope.toCurrency = 'EUR';
        $scope.convertedAmount = 0;

        $scope.convert = function() {
          const apiKey = 'your_api_key_here'; // Replace with a valid API key for a real API
          const url = `https://api.exchangerate-api.com/v4/latest/${$scope.fromCurrency}`;

          $http.get(url).then(function(response) {
            const rate = response.data.rates[$scope.toCurrency];
            $scope.convertedAmount = $scope.amount * rate;
          }, function(error) {
            console.error('Error fetching exchange rate:', error);
          });
        };
      }]);
  </script>
</body>
</html>
