# my-file
!pip install transformers==4.31.0 tqdm

from transformers import pipeline, AutoModelForSeq2SeqLM, AutoTokenizer, AutoConfig
from tqdm.auto import tqdm
import logging
import os
import json

logging.basicConfig(level=logging.ERROR)

# Cache for storing generated results
generated_text_cache = {}

def generate_text(prompts, model_name="gpt2", max_length=50, num_return_sequences=1, **kwargs):
    """Generates text, utilizing caching and advanced options."""
    results = []
    for prompt in tqdm(prompts, desc="Generating Text"):
        cache_key = (prompt, model_name, max_length, num_return_sequences, tuple(kwargs.items()))
        if cache_key in generated_text_cache:
            results.append(generated_text_cache[cache_key])
            continue

        try:
            # Model loading and generation (similar to previous versions)
            # ... (code for model loading and generation) ...

            result = {
                "prompt": prompt,
                "generated_text": generated_text[0]['generated_text'] if generated_text else "Error: Could not generate text."
            }
            results.append(result)
            generated_text_cache[cache_key] = result  # Store in cache
        except Exception as e:
            results.append({"prompt": prompt, "generated_text": f"Error: {e}"})

    return results

# ... (Rest of the code: user input, parameter handling, etc.) ...

# Output saving (optional)
if input("Save output to a file? (y/n): ").lower() == 'y':
    output_filename = input("Enter filename (e.g., output.json): ")
    with open(output_filename, 'w') as f:
        json.dump(results, f, indent=4)
    print(f"Output saved to {output_filename}")
