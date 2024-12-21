# 替代規則執行工作流 / Workflow for Executing Substitution Rules

本文件提供本專案的詳細說明，包含中文和英文兩個版本。請根據需要查閱相應語言的部分。

This document provides a detailed description of the project, including both Chinese and English versions. Please refer to the appropriate language section as needed.

---

## 目錄 / Table of Contents

1. [中文版](#中文版)
2. [English Version](#english-version)

---

## 中文版

# 替代規則執行工作流

本專案透過替代規則對逐字稿執行自動化校正，以提升文稿的準確性與可讀性。下為詳細的工作流程說明。

## 目錄

- [工作流程](#工作流程)
  - [步驟1：錯字整理](#步驟1錯字整理)
  - [步驟2：生成替代規則代碼](#步驟2生成替代規則代碼)
  - [步驟3：應用替代規則](#步驟3應用替代規則)
  - [步驟4：重複校正](#步驟4重複校正)
- [使用說明](#使用說明)
- [範例](#範例)
- [貢獻](#貢獻)
- [許可](#許可)

## 工作流程

### 步驟1：錯字整理

將轉寫文成的文稿按每50分鐘的時間戳記分段，並在 Google AI Studio 上使用 Gemini 2.0 Thinking 模型整理出錯字表。

- **系統提示**：

  ```plaintext
  幫我找出上述民法課程逐字稿中所有最明顯的錯字。
  以表格列出，顯示四個欄位：大致的時間戳記、錯誤的詞彙、建議的詞彙（如建議詞彙相同，請相互排在表格的附近）、說明（比如上下文都與某主題&概念相關）
  不要調整原本句子的語氣，校正的方向必須忠於逐字稿原文，主要找出一些容易發生口誤的地方‧‧‧
  ```

### 步驟2：生成替代規則代碼

將生成的錯字表貼到 Excel 上，僅提取「錯誤的詞彙」與「建議的詞彙」，並使用 GPT-4 製作成替代規則代碼。

- **範例代碼**：

  ```python
  substitution_rules = [
      (r"XX", "OO"),
      # 其他替代規則
  ]
  ```

### 步驟3：應用替代規則

透過 GPT-4 的代碼解析器，直接對上傳的 txt 逐字稿進行取代。

### 步驟4：重複校正

重複以上步驟，直至替代掉所有錯漏部分。

## 使用說明

1. **準備逐字稿**：確保逐字稿為 TXT 格式，並按照每50分鐘分段，包含時間戳記。(按經驗法則，8092個output_token約可一次性校正約50分鐘分量的中文錯別字)
2. **錯字整理**：
   - 上傳逐字稿至 Google AI Studio。
   - 使用 Gemini 2.0 Thinking 模型，並輸入上述系統提示，生成錯字表。
3. **生成替代規則**：
   - 將錯字表匯入 Excel。
   - 提取「錯誤的詞彙」與「建議的詞彙」兩欄。
   - 使用 GPT-4 將提取的詞彙轉換為替代規則代碼。
4. **應用替代規則**：
   - 使用生成的替代規則代碼，對 TXT 逐字稿進行批量替換。
5. **驗證結果**：
   - 檢查替換後的逐字稿，確保所有錯誤已被修正。
   - 如有遺漏，返回步驟1進行再次校正。

## 範例

以下為一個簡單的範例，展示如何使用替代規則代碼進行文本替換。

```python
import re

# 替代規則
substitution_rules = [
    (r"XX", "OO"),
    # 其他規則
]

def apply_substitutions(text, rules):
    for pattern, replacement in rules:
        text = re.sub(pattern, replacement)
    return text

# 讀取逐字稿
with open('transcript.txt', 'r', encoding='utf-8') as file:
    content = file.read()

# 應用替代規則
corrected_content = apply_substitutions(content, substitution_rules)

# 保存校正後的逐字稿
with open('corrected_transcript.txt', 'w', encoding='utf-8') as file:
    file.write(corrected_content)
```

## 貢獻

歡迎任何形式的貢獻！

## 許可

本專案採用 [MIT 許可](LICENSE)。

---

## English Version

# Workflow for Executing Substitution Rules

This project aims to automate the correction of typos in the transcripts using substitution rules, enhancing the accuracy and readability of the documents. Below is a detailed description of the workflow.

## Table of Contents

- [Workflow](#workflow)
  - [Step 1: Error Compilation](#step-1-error-compilation)
  - [Step 2: Generate Substitution Rules Code](#step-2-generate-substitution-rules-code)
  - [Step 3: Apply Substitution Rules](#step-3-apply-substitution-rules)
  - [Step 4: Repeat Correction](#step-4-repeat-correction)
- [Usage Instructions](#usage-instructions)
- [Example](#example)
- [Contributing](#contributing)
- [License](#license)

## Workflow

### Step 1: Error Compilation

Segment the transcribed text by approximately 50-minute timestamps and use the Gemini 2.0 Thinking model in Google AI Studio to compile a list of typos.

- **System Prompt**:

  ```plaintext
  Help me identify all the most obvious typos in the above Civil Law course transcript.
  Present them in a table with four columns: approximate timestamp, incorrect term, suggested term (if the suggested terms are identical, please place them near each other in the table), and explanation (e.g., context related to a specific topic & concept).
  Do not adjust the original sentence's tone; the correction direction must remain faithful to the original transcript. Focus on identifying areas prone to verbal mistakes...
  ```

### Step 2: Generate Substitution Rules Code

Paste the generated error table into Excel, extract only the "Incorrect Term" and "Suggested Term" columns, and use GPT-4 to create substitution rules code.

- **Sample Code**:

  ```python
  substitution_rules = [
      (r"XX", "OO"),
      # Additional substitution rules
  ]
  ```

### Step 3: Apply Substitution Rules

Use GPT-4's code parser to directly perform replacements on the uploaded TXT transcript.

### Step 4: Repeat Correction

Repeat the above steps until all errors and omissions are corrected.

## Usage Instructions

1. **Prepare Transcript**: Ensure the transcript is in TXT format, segmented approximately every 50 minutes, and includes timestamps.
2. **Error Compilation**:
   - Upload the transcript to Google AI Studio.
   - Use the Gemini 2.0 Thinking model with the above system prompt to generate the error table.
3. **Generate Substitution Rules**:
   - Import the error table into Excel.
   - Extract the "Incorrect Term" and "Suggested Term" columns.
   - Use GPT-4 to convert the extracted terms into substitution rules code.
4. **Apply Substitution Rules**:
   - Use the generated substitution rules code to perform bulk replacements on the TXT transcript.
5. **Verify Results**:
   - Check the corrected transcript to ensure all errors have been fixed.
   - If any omissions are found, return to Step 1 for further correction.

## Example

Below is a simple example demonstrating how to use substitution rules code for text replacement.

```python
import re

# Substitution Rules
substitution_rules = [
    (r"XX", "OO"),
    # Additional rules
]

def apply_substitutions(text, rules):
    for pattern, replacement in rules:
        text = re.sub(pattern, replacement)
    return text

# Read transcript
with open('transcript.txt', 'r', encoding='utf-8') as file:
    content = file.read()

# Apply substitution rules
corrected_content = apply_substitutions(content, substitution_rules)

# Save corrected transcript
with open('corrected_transcript.txt', 'w', encoding='utf-8') as file:
    file.write(corrected_content)
```

## Contributing

Contributions are welcome in any form! 

## License

This project is licensed under the [MIT License](LICENSE).
