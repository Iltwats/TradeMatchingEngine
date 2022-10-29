cmake_minimum_required(VERSION 3.23)
project(TradeMatchingEngine)

set(CMAKE_CXX_STANDARD 17)

add_executable(TradeMatchingEngine common.h main.cpp order_attribute.h order_factory.cpp order_factory.h order_input.cpp order_input.h order_trade.cpp order_trade.h trade_engine.cpp trade_engine.h)
