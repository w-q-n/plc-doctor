# plc-Doctor
plc故障排除
# ==================================
# PLC Doctor - AI智能故障诊断助手
# Version 0.1
# ==================================

import streamlit as st

# 页面配置
st.set_page_config(
    page_title="PLC Doctor",
    page_icon="⚡",
    layout="centered"
)

# 页面标题
st.title("⚡ PLC Doctor")
st.subheader("智能PLC故障诊断助手")

st.write("欢迎使用 PLC Doctor")
st.write("描述你的设备故障，获取初步排查建议。")

# PLC型号选择
plc_model = st.selectbox(
    "请选择PLC型号",
    [
        "三菱 FX3U",
        "三菱 FX5U",
        "西门子 S7-1200",
        "西门子 S7-200 SMART"
    ]
)

# 故障描述
fault = st.text_area(
    "请描述你的故障现象",
    placeholder="例如：PLC运行正常，但是Y0没有输出..."
)

# 故障诊断函数
def diagnose(model, fault):

    fault = fault.lower()

    if "err" in fault or "错误" in fault:

        return """
        【初步诊断】

        可能原因：
        1. PLC检测到程序或硬件错误
        2. 参数配置异常
        3. 外部设备通信异常

        建议排查：
        1. 查看PLC诊断信息
        2. 记录具体错误代码
        3. 检查近期程序修改记录

        注意：需要结合具体型号和错误代码进一步判断。
        """

    elif "y0" in fault or "没有输出" in fault:

        return """
        【初步诊断】

        可能原因：
        1. 程序没有满足Y0导通条件
        2. 输出模块异常
        3. COM公共端接线问题
        4. 外部负载或供电异常

        建议排查：
        1. 查看Y0输出指示灯
        2. 检查梯形图逻辑
        3. 核对输出类型
        4. 检查外部接线

        注意：检查电路前应执行断电、验电等安全措施。
        """

    elif "通信" in fault or "通讯" in fault:

        return """
        【初步诊断】

        可能原因：
        1. 通信参数不匹配
        2. 通信线路异常
        3. 设备地址配置错误
        4. 通信设备故障

        建议排查：
        1. 核对波特率
        2. 检查通信协议
        3. 检查接线
        4. 查看通信错误代码
        """

    else:

        return """
        暂时无法准确识别该故障。

        请补充以下信息：
        1. PLC具体型号
        2. 故障指示灯状态
        3. 故障发生前设备做了什么
        4. 输入输出状态
        5. 是否有具体错误代码

        信息越完整，诊断越有针对性。
        """


# 诊断按钮
if st.button("开始故障诊断"):

    if fault.strip() == "":
        st.warning("请先输入故障现象")

    else:
        result = diagnose(plc_model, fault)

        st.success("诊断完成")

        st.markdown(result)

        st.caption(
            "PLC Doctor 仅提供辅助诊断建议，"
            "不能代替专业人员现场检测。"
        )


# 页面底部
st.divider()

st.caption("PLC Doctor © 2026")
st.caption("让工业维修更加简单")
