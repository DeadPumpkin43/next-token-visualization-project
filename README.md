# Next Token Lab (GPT-2 edition)

A single-page tool. Type a sentence, see the model's raw logits and probabilities
for the next token, and drag the temperature from 0.1 to 2.

## Run it

Browsers block some features on `file://` pages, so serve the folder:

    python3 -m http.server 8000

then open http://localhost:8000

## Put it on GitHub Pages

Copy `index.html` to the root of your Pages branch (or to `/docs`) and push.
No build step.

## How it works

- Transformers.js (loaded from jsDelivr) runs GPT-2 in the browser via WebAssembly.
- The model files download from the Hugging Face Hub on first visit
  (roughly 90 MB for distilgpt2, 130 MB for gpt2) and are cached by the browser.
- The page reads the logits at the last position, divides by temperature,
  and applies softmax over the full 50,257-token vocabulary.

## Tweaks

- Add more models: add an `<option>` to the `#model` select. Use repos under
  `Xenova/` that contain ONNX weights.
- Faster inference on supported browsers: in `AutoModelForCausalLM.from_pretrained`,
  add `device: "webgpu"` next to `dtype`.
- Limits: prompts are cut at 256 tokens (`MAX_TOKENS`).
