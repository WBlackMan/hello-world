# hello-world

I like angularJs.

angular.module("myApp",[]).controller("TradeController",function ($scope) {
    $scope.cart = [
        {
            id:123,
            CpName:"iphone5s",
            BuyNum:30,
            CpPrice:3000,
        },
        {
            id:2234,
            CpName:"iphone6",
            BuyNum:15,
            CpPrice:3600,
        },
        {
            id:3543,
            CpName:"iphone6s",
            BuyNum:5,
            CpPrice:4500,
        },
        {
            id:4765,
            CpName:"iphone7",
            BuyNum:23,
            CpPrice:5900,
        },
        {
            id:5879,
            CpName:"iphone4",
            BuyNum:45,
            CpPrice:2000,
        }
    ];
    // 购买总价
    $scope.BuyZongPrice = function () {
        var ZongPrice = 0;
        angular.forEach($scope.cart,function (item) {
            ZongPrice += item.BuyNum * item.CpPrice;
        })
        return ZongPrice;
    }
    // 购买总数量
    $scope.BuyZongNum = function () {
        var ZongNum = 0;
        angular.forEach($scope.cart,function (item) {
            ZongNum += parseInt(item.BuyNum);
        })
        return ZongNum;
    }
 // 删除行
    $scope.remove = function (id) {
        var index = -1;
        angular.forEach($scope.cart,function (item,key) {
            // console.log(key);
            if (item.id === id) {
                index = key;
            }
        })

        if(index !== -1) {
            $scope.cart.splice(index,1);
        }
    }

    // 先找到一个元素的索引
    var findIndex = function (id) {
        var index = -1;
        angular.forEach($scope.cart,function (item,key) {
            if (item.id === id) {
                index = key;
                return;
            }
        })
        return index;
    }
    // 购买数量做 加加
    $scope.add = function (id) {
        var index = findIndex(id);
        if(index !== -1) {
            ++$scope.cart[index].BuyNum;
        }
    }

    // 购买数量做 减减
    $scope.down = function (id) {
        var index = findIndex(id);
        if(index !== -1) {
            if($scope.cart[index].BuyNum > 1) {
                --$scope.cart[index].BuyNum;
            }else {
                if(confirm("您确定从购物车删除它？")) {
                    $scope.remove(id);
                }
            }
        }
    }

    // 如果购买数量Input中输入 负数的时候
    // 我们需要对$scope.cart做个监听 $watch
    $scope.$watch("cart",function (newValue, oldValue) {
        angular.forEach(newValue,function (item,key) {
            if(item.BuyNum < 1){
                if(confirm("您确定从购物车删除它？")) {
                    $scope.remove(item.id);
                }else {
                    item.BuyNum = oldValue[key].BuyNum;
                }
            }
        })
    },true)
})
