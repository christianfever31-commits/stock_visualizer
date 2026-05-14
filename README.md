import streamlit as st
import yfinance as yf
import pandas as pd
import plotly.graph_objects as go

# Page Config
st.set_page_config(page_title="Stock Visualizer", layout="wide")
st.title("📈 Ezenwa's Stock Market Visualizer")

# Input Settings in Sidebar
st.sidebar.header("Input Settings")
ticker = st.sidebar.text_input("Stock Ticker", value="AAPL")
period = st.sidebar.selectbox("Period", ["1mo", "6mo", "1y", "5y", "max"])

# Fetch Data
data = yf.download(ticker, period=period)

if not data.empty:
    # Create Plotly Candlestick Chart
    fig = go.Figure(data=[go.Candlestick(
        x=data.index,
        open=data['Open'],
        high=data['High'],
        low=data['Low'],
        close=data['Close']
    )])

    fig.update_layout(
        template="plotly_dark",
        xaxis_rangeslider_visible=False,
        margin=dict(l=10, r=10, t=10, b=10)
    )

    st.plotly_chart(fig, use_container_width=True)

    # Show Latest Stats
    last_price = data['Close'].iloc[-1]
    st.write(f"**Latest Price:** ${last_price:.2f}")
else:
    st.error("Invalid Ticker or no data found. Please use symbols like AAPL, TSLA, or BTC-USD.")
