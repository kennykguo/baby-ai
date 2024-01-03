# Newhacks-2023: Baby AI

Looking to push the boundaries of accessible education, introducing **Baby AI**

Baby AI is a GPT-powered educational aid that creates customized problem sets for all your courses. Just provide a PDF of your class notes and Baby will generate you a problem set in no time! 

## Usage

While we have a frontend UI created, it is not yet deployed to the internet. Unfortunately usage is a little complicated right now but we'll provide the steps anyway:

### Localhost (probably the easiest)

  1. Navigate to and copy `/babyv2/`
  2. run `flask --app app run`
  3. Upload your pdf, enjoy!

### Using the .ipynb file (if you want more control)

  1. Navigate to and copy `/extraction_test/testing.ipynb` (the name is deceiving, and I am lazy)
  2. Run a notebook instance
  3. Fill in your own information, specify file paths, enjoy!

### With PyInstaller (a lot more work)

  1. Navigate to and copy `/baby-executable/baby-executable.py`, you can do this however you want, the only important part is the extraction and prompt process.
  3. Add your own OpenAI API key (don't be like Jason and hard code it before accidentally pushing to a public repository).
  4. From the directory containing `baby-executable.py`, run `python3 -m PyInstaller baby-executable.py`.
  5. run: `./dist/baby-executable /path/to/pdf/file #_of_multiple_choice(default=2) #_of_long_answer(default=3) model(default=gpt-3.5-turbo)`

## How does it work?

Baby AI currently uses GPT-3.5-Turbo to power its problem set generation. Each PDF is parsed and its text is extracted, before being supplied to GPT as a prompt. The system prompt is set up as follows:

```
You are a problem set writer. Given a set of class notes, compile a problem set based on the relevant content. 
The output must be in GFM format so it can be easily exported as a PDF file. 
The conversion will happen through Pandoc, ensure no text overflows.
Show the problem set at the top, and the solution set at the bottom.
The problem set must contain {num_mcq} multiple choice questions and {num_long} long answer questions.
```

The plain text output is then processed and formatted into a nice PDF file through Pandoc!

## Why is this useful? (+ inspiration behind Baby)

As stated above, Baby AI and the team behind it are looking to push accessible education by providing educational resources while only requiring an internet connection! (this is not fully true yet, requests are currently being made through a team members OpenAI account)

Created with [LearnFreely](https://learnfreely.ca), inspiration for Baby AI came as a means to address a gap in LearnFreely's mentorship operations. LearnFreely was set up to provide free mentorships for high school students in areas that generally lacked educational support. As students from around the province poured in, we found that we lacked the physical resources to provide the best help possible. As such, some keen mentors decided to take the initative and begin writing problem sets by themselves, creating the spark that (with the help of NewHacks) would become Baby AI!

Though very much still in its proof-of-concept phases, we hope to one day deploy Baby AI for use by LearnFreely's mentors to continue pushing our mission and providing others with services unavailable to us. 

## What's next?

Baby AI still has a long way to go.

For starters, we are currently powered through GPT-3.5-Turbo, a model that has been outperformed by certain competitors. The model lacks fine-tuning leading to unreliable results that do not exactly follow the prompt. Experimentation with GPT-4 has shown promising results, with the quality of problems and formatting being significantly higher, however the cost makes a full GPT-4-powered deployment currently unfeasible. Llama 2 is another route we have been considering, however the large difference in method of API calls (and just general lack of experience) made the team decide to stick to GPT for NewHacks purposes. 

We are currently restricted to digitally written PDF files. This is a large limitation since many student notes are hand written, thus a more comprhensive data extraction method should be explored going forwards. Computer vision related topics were discussed in the group, but deemed unfit for the NewHacks timeline.

The API calls are also quite inefficient. Our text extraction lacks nuance and simply extracts every character on a given PDF into a Python string which is used to prompt GPT. This leads to token restrictions and we must ensure the class notes being used are small enough to stay under the 4096 token limit (~3000 words). In testing, we noticed that much of the text in the notes is not useful in providing context to the underlying subject, therefore more rigorous text encoding methods should be explored. 

With the new LearnFreely website currently in progress, the LF team is looking to integrate new features into our daily operations. As mentioned, we hope to eventually implement Baby AI into LearnFreely, and this could be done through the new site.
