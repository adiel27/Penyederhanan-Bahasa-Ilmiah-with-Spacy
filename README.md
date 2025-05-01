# Penyederhanan-Bahasa-Ilmiah-with-Spacy
Menyederhanakan bahasa ilmiah agar mudah dipahami

Masukkan ini ke Google Colab
!python -m spacy download xx_ent_wiki_sm
import spacy # Import the spacy module before using it
nlp = spacy.load("xx_ent_wiki_sm")

Ini untuk membuat kamus patokannya
simplification_dict = {
    "implementasi": "memakai",
    "eksplorasi": "mencari tahu",
    "identifikasi": "menemukan",
    "evaluasi": "menilai",
    "literasi": "bisa membaca dan memahami",
    "skill": "kemampuan"
}

def simplify_text(text):
    doc = nlp(text)
    simplified_words = [simplification_dict[token.text] if token.text in simplification_dict else token.text for token in doc]
    return " ".join(simplified_words)

Ini untuk menghilangkan tanda baca

import string

def preprocess_text(text):
    text = text.lower()
    text = text.translate(str.maketrans("", "", string.punctuation))  # Hilangkan tanda baca
    return text

Ini untuk penyederhanaannya

def simplify_text(text):
    doc = nlp(text)
    simplified_words = []
    
    for token in doc:
        if token.text in simplification_dict:
            simplified_words.append(simplification_dict[token.text])  # Ganti dengan kata sederhana
        else:
            simplified_words.append(token.text)  # Biarkan kata lain tetap sama

    return " ".join(simplified_words)

Ini untuk menjalankannya

import spacy
from spacy.matcher import PhraseMatcher

nlp = spacy.load("xx_ent_wiki_sm")  # Model multilingual jika tidak ada model bahasa Indonesia

# Daftar kata dan frasa ilmiah yang ingin dideteksi
academic_phrases = [
    "implementasi teknologi",
    "implementasi pendidikan",
    "eksplorasi data",
    "identifikasi pola",
    "evaluasi sistem",
    "literasi digital",
    "signifikan",
    "skill"
]
# Buat PhraseMatcher
matcher = PhraseMatcher(nlp.vocab)
patterns = [nlp(text) for text in academic_phrases]
matcher.add("ACADEMIC_PHRASES", patterns)

# Fungsi deteksi multi-kata
def detect_academic_language(text):
    doc = nlp(text)
    matches = matcher(doc)
    detected = [doc[start:end].text for match_id, start, end in matches]
    return detected if detected else "Tidak ada kata atau frasa ilmiah yang terdeteksi."

# Contoh penggunaan
text = "implementasi teknologi dalam pendidikan sangat penting dalam literasi yang secara signifikan bisa meningkatkan skill."
print("Frasa Ilmiah yang Terdeteksi:", detect_academic_language(text))
print("Kalimat Sederhana:", simplify_text(text))

# Anda bisa memperluas dan memperbanyak kata ilmiah di model ini dengan memasukkannya ke bagian dictionary 
