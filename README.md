def bnp_strategy(initial_cash, prices):
    cash = initial_cash
    stocks = 0
    for price in prices:
        if cash >= price:
            buy_count = cash // price  # 구매 가능한 주식 수
            stocks += buy_count  # 주식 매수
            cash -= buy_count * price  # 주식을 사서 남은 현금
    return cash + stocks * prices[-1]  # 마지막 날 주식 가격으로 자산 계산

def timing_strategy(initial_cash, prices):
    cash = initial_cash
    stocks = 0
    for i in range(2, len(prices)):
        if prices[i] > prices[i-1] and prices[i-1] > prices[i-2]:  # 3일 연속 상승
            if stocks > 0:  # 주식 보유 중이면 매도
                cash += stocks * prices[i-1]
                stocks = 0
        elif prices[i] < prices[i-1] and prices[i-1] < prices[i-2]:  # 3일 연속 하락
            if cash >= prices[i]:  # 주식 매수 가능하면 매수
                buy_count = cash // prices[i]
                stocks += buy_count
                cash -= buy_count * prices[i]
    return cash + stocks * prices[-1]  # 마지막 날 주식 가격으로 자산 계산

def main():
    # 입력 받기
    initial_cash = int(input())  # 준현이와 성민이에게 주어진 현금
    prices = list(map(int, input().split()))  # 주식 가격 리스트

    # BNP 전략 실행
    bnp_asset = bnp_strategy(initial_cash, prices)

    # TIMING 전략 실행
    timing_asset = timing_strategy(initial_cash, prices)

    # 자산 비교하여 출력
    if bnp_asset > timing_asset:
        print("BNP")
    elif bnp_asset < timing_asset:
        print("TIMING")
    else:
        print("SAMESAME")

if __name__ == "__main__":
    main()
