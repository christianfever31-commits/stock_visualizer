import streamlit as st
import yfinance as yf
import pandas as pd
import plotly.graph_objects as go

# --- Page Config ---
st.set_page_config(page_title="Stock Visualizer", layout="wide")
st.title("📈 Ezenwa's Stock Market Visualizer")

# --- Input Settings ---
st.sidebar.header("Input Settings")
ticker = st.sidebar.text_input("Stock Ticker", value="AAPL")
period = st.sidebar.selectbox("Period", ["1mo", "6mo", "1y", "5y", "max"])

# --- Fetch Data ---
data = yf.download(ticker, period=period)

if not data.empty:
    # Create Plotly Chart
    fig = go.Figure(data=[go.Candlestick(
        x=data.index,
        open=data['Open'],
        high=data['High'],
        low=data['Low'],
        close=data['Close']
    )])
    
    fig.update_layout(template="plotly_dark", xaxis_rangeslider_visible=False)
    st.plotly_chart(fig, use_container_width=True)
    
    # Latest Price
    st.write(f"**Latest Price:** ${data['Close'].iloc[-1]:.2f}")
else:
    st.error("Invalid Ticker or No Data Found.")
