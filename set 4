import requests
import random
from datetime import datetime

class DictionaryAPI:
    def __init__(self):
        self.base_url = "https://api.dictionaryapi.dev/api/v2/entries/en"
        self.word_list = [
            # Academic & Scientific
            "quantum", "paradigm", "hypothesis", "synthesis", "analysis",
            "theory", "empirical", "cosmic", "atomic", "molecular", "neural",
            
            # Technology & Computing
            "algorithm", "digital", "virtual", "interface", "quantum", "cyber",
            "neural", "binary", "encryption", "bandwidth", "protocol",
            
            # Philosophy & Abstract
            "metaphysical", "paradox", "existential", "ethereal", "sublime",
            "infinity", "consciousness", "philosophy", "metaphor", "abstract",
            
            # Nature & Environment
            "ecosystem", "biodiversity", "sustainable", "atmospheric", "geological",
            "botanical", "oceanic", "terrestrial", "celestial", "aurora",
            
            # Art & Culture
            "aesthetic", "renaissance", "baroque", "symphony", "theatrical",
            "artistic", "cultural", "classical", "creative", "innovative",
            
            # Psychology & Behavior
            "cognitive", "behavioral", "psychological", "intuitive", "empathy",
            "perception", "consciousness", "motivation", "personality", "emotional",
            
            # Society & Relationships
            "diplomatic", "collaborative", "humanitarian", "sociological",
            "anthropological", "cultural", "communal", "democratic", "ethical",
            
            # Business & Economics
            "economic", "strategic", "innovative", "entrepreneurial", "commercial",
            "financial", "industrial", "corporate", "sustainable", "operational"
        ]
    
    def get_random_word(self):
        return random.choice(self.word_list)
    
    def get_word_data(self, max_attempts=5):
        for attempt in range(max_attempts):
            word = self.get_random_word()
            print(f"\nTrying word {attempt + 1}/{max_attempts}: {word}")
            
            try:
                url = f"{self.base_url}/{word.lower()}"
                response = requests.get(url)
                
                if response.status_code == 200:
                    data = response.json()
                    if self._is_good_definition(data):
                        print(f"Found good definition for: {word}")
                        return word, data
                    print("Definition wasn't complete enough, trying another word...")
                else:
                    print(f"Word not found in dictionary, trying another...")
                    
            except requests.exceptions.RequestException as e:
                print(f"Error making request: {e}")
        
        return self.get_fallback_word()

    def _is_good_definition(self, data):
        if not data or not isinstance(data, list):
            return False
        
        word_data = data[0]
        total_definitions = sum(len(meaning.get('definitions', [])) 
                              for meaning in word_data.get('meanings', []))
        return total_definitions >= 2

    def get_fallback_word(self):
        fallback_words = ["innovation", "analysis", "research", "development", "technology"]
        for word in fallback_words:
            url = f"{self.base_url}/{word.lower()}"
            try:
                response = requests.get(url)
                if response.status_code == 200:
                    return word, response.json()
            except:
                continue
        return None, None

def format_word_info(word, data):
    if not data or not isinstance(data, list):
        return "No data available"
    
    output = []
    word_data = data[0]
    
    # Header
    output.append("\n" + "="*60)
    output.append(f"WORD OF THE DAY - {datetime.now().strftime('%B %d, %Y')}")
    output.append("="*60)
    
    # Word and pronunciation
    output.append(f"\n📚 WORD: {word_data['word'].upper()}")
    if 'phonetic' in word_data and word_data['phonetic']:
        output.append(f"🗣️ Pronunciation: {word_data['phonetic']}")
    
    # Definitions and usage
    for meaning in word_data['meanings']:
        output.append(f"\n📖 Part of Speech: {meaning['partOfSpeech'].upper()}")
        
        for i, definition in enumerate(meaning['definitions'], 1):
            output.append(f"\nDefinition {i}:")
            output.append(f"   {definition['definition']}")
            
            if 'example' in definition:
                output.append(f"   Example: \"{definition['example']}\"")
        
        if meaning.get('synonyms'):
            output.append(f"\n✨ Synonyms: {', '.join(meaning['synonyms'][:5])}")
            
        if meaning.get('antonyms'):
            output.append(f"❌ Antonyms: {', '.join(meaning['antonyms'][:5])}")
    
    output.append("\n" + "="*60)
    return "\n".join(output)

def main():
    print("Initializing Dictionary API client...")
    client = DictionaryAPI()
    
    word, word_data = client.get_word_data()
    if word and word_data:
        print(format_word_info(word, word_data))
    else:
        print("\nFailed to get word information.")

if __name__ == "__main__":
    main()
