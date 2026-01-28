import streamlit as st

# --- 連携を強化するための設定 ---
st.set_page_config(page_title="野球布陣アプリ", layout="centered")

# アプリが起動したときに、データを「スマホのメモリ」にしっかり固定する
if 'players_list' not in st.session_state:
    st.session_state.players_list = [
        {"pos": "投手", "name": "はやさかひろや"},
        {"pos": "捕手", "name": "さとうたける"},
        {"pos": "一塁", "name": "たかはしはじめ"},
        {"pos": "二塁", "name": "きくちてつや"},
        {"pos": "三塁", "name": "たなかりん"},
        {"pos": "遊撃", "name": "おいかわさとし"},
        {"pos": "左翼", "name": "いいだしんじ"},
        {"pos": "中堅", "name": "はやかわかい"},
        {"pos": "右翼", "name": "はやしたくや"},
    ]

# --- 画面表示 ---
st.title("⚾ チーム布陣")

# 現在のメンバーをダイヤモンド状に表示
cols = st.columns(3)
for i, p in enumerate(st.session_state.players_list):
    # 配置ロジック（簡易版）
    with st.container():
        st.write(f"**{p['pos']}**: {p['name']}")

st.divider()

# --- 交代の操作 ---
st.subheader("選手交代の記録")
out_member = st.selectbox("退く選手", [p['name'] for p in st.session_state.players_list])
in_name = st.text_input("代わりに入る選手の名前（またはベンチから選択）")

if st.button("交代を確定してスマホに保存"):
    if in_name:
        # メモリ内のデータを直接書き換える
        for p in st.session_state.players_list:
            if p['name'] == out_member:
                p['name'] = in_name
        st.success(f"{out_member} ➔ {in_name} に変更しました！")
        # 画面を強制的に再描画して連携を即時反映させる
        st.rerun()
