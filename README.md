"""
Token Analysis Tool for LLM Prompts
-----------------------------------
This tool analyzes how text gets converted into tokens for LLMs (like GPT-4).
It shows token counts, word-level breakdowns, and identifies words that use multiple tokens.

Use case: Understanding token consumption in your prompts before using paid APIs.
"""

import tiktoken
import re

"""
Initialize the token analyzer with a specific encoding.
Args: encoding_name: Tokenizer encoding name 
(default: "cl100k_base" for GPT-4/GPT-3.5)
Other options: "p50k_base", "r50k_base", or "gpt2"
"""
encoding = tiktoken.get_encoding("cl100k_base")

"""
# Sample text
text = """
Act as an English as a foreign language teacher. 
Can we have a short conversation about my favorite meal? 
Ask me one question at a time to keep the conversation going. 
In your responses, gently correct any grammar or vocabulary mistakes I make.
"""
"""

# Get text input from user
print("Enter your text (press Enter twice to finish):")
lines = []
while True:
    line = input()
    if line == "" and len(lines) > 0 and lines[-1] == "":
        break
    lines.append(line)

text = "\n".join(lines).strip()

#  Extract individual words from text using regex.
words = re.findall(r"\b\w+\b", text)

# Encode full text
tokens = encoding.encode(text)

# Print basic statistic
print("\n" + "=" * 60)
print("📊 BASIC STATISTICS")
print("=" * 60)
print(f"\n📝 FULL TEXT:\n")
print(analysis["original_text"])
print("\n" + "=" * 60)
print(f"📈 STATISTICS:")
print(f"   • Total Words: {len['words']:,}")
print(f"   • Total Tokens: {len['tokens']:,}")

# Analyze how a single word is tokenized.
print("WORD – TOKEN ANALYSIS\n")

for word in words:
	# Encode each word separately
    	word_tokens = encoding.encode(word)
	# Count token for this word
	token_count = len(word_tokens)

	# Decode tokens to show splitting
	split_parts = {encoding.decode([t]) for t in word_tokens}

	print(f"Word: '{word}'")
    	print(f"Token Count: {token_count}")
    	print(f"Token IDs: {word_tokens}")
    	print(f"Split Into: {split_parts}")

    	# Highlight long tokenized word
	if token_count >= 2:
        print("This word uses MULTIPLE tokens")
    	print("-" * 40)

# Show only words with more than 2 words 
print("WORDS USING 2 OR MORE TOKENS\n")

for word in words:
   	 word_tokens = encoding.encode(word)
    
   	if len(word_tokens) >= 2:
        	split_parts = {encoding.decode([t]) for t in word_tokens}
        	print(f"'{word}'  -  {len(word_tokens)} tokens")
        	print(f"Split Parts: {split_parts}")
        	print("=" * 70)
