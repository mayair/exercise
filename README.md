import random
#from urllib import urlopen
import sys

#WORD_URL = "http://learncodethehardway.org/words.txt"
#create an empty list to store words
WORDS = []

#create a dict named PHRASES
PHRASES = {
	"Class%%%(%%%):":
		"Make a class named %%% that is-a %%%",
	"Class %%%(object):\n\tdef __init__(self,***)":
		"Class %%% has-a __init__ that takes self and *** parameters.",
	"Class %%%(object):\n\tdef ***(self, @@@)":
		"Class %%% has-a function named *** that takes self and @@@ parameters.",
	"*** = %%%()":
		"Set *** to an instance of class %%%.",
	"***.***(@@@)":
		"From *** get the *** function, and call it with parameters self, @@@.",
	"***.*** = '***'":
		"From *** get the *** attribute and set it to '***'."
	}
	
# do they want to drill phrases first
#DO NOT UNDERSTAND
if len(sys.argv) == 2 and sys.argv[1] == "english":
	PHRASE_FIRST = True
else:
	PHRASE_FIRST = False
	
# load up the words from the website
#look for word in URL,strip word and add to WORDS list
for word in open("words.txt").readlines():
	WORDS.append(word.strip())
	
	
def convert(snippet, phrase):
	class_names = [w.capitalize() for w in
		random.sample(WORDS, snippet.count("%%%"))]
	other_names = random.sample(WORDS, snippet.count("***"))
	#create empty list called results & param_names
	results = []
	param_names = []
	
	for i in range(0,snippet.count("@@@")):
		#pick a number among 1 ,2,3
		param_count = random.randint(1,3)
		#pick random number of words picked just now from WORDS and join them together with ",", 
		#then append them to list param_names
		param_names.append(",".join(random.sample(WORDS,param_count)))
	
	for sentence in snippet, phrase:
		result = sentence[:]
	
		#fake class names
		for word in class_names:
			result = result.replace("%%%", word, 1)
		
		#fake other names
			for word in other_names:
				result = result.replace("***", word,1)
				
		#fake parameter lists
		for word in param_names:
			result = result.replace("@@@",word,1)
		
		results.append(result)
		
	return results
	
#keep going until they hit CTRL-D
try:
	while True:
		# print keys
		snippets = PHRASES.keys()
		random.shuffle(snippets)
		
		for snippet in snippets:
			phrase = PHRASES[snippet]
			question, answer = convert(snippet,phrase)
			if PHRASE_FIRST:
				question , answer = answer, question
			
			print question
			
			raw_input(">")
			print "ANSWER: %s\n\n" % answer

except EOFError:
	print "\nBye"		
