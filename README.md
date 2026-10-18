# Transly speech packs

This repository hosts the on-device models that the Transly app downloads.
The app uses a speech pack when the phone's own recognizer cannot recognize a language offline.
It uses the voice pack when the phone's own text-to-speech engine cannot read a language aloud.
Each pack runs on [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx), on the phone CPU.

| Release | Model | Languages | Licence |
|---|---|---|---|
| `indic-v1` | [AI4Bharat IndicConformer](https://github.com/AI4Bharat/IndicConformerASR), modified | Bengali, Marathi, Tamil, Telugu, Urdu (one pack each) | MIT |
| `general-v1` | [OpenAI Whisper small](https://github.com/openai/whisper), int8 | Chinese (Mandarin), Korean, Russian, Portuguese, Indonesian, Turkish, Vietnamese (one shared pack) | MIT |
| `general-v1` | [Meta Omnilingual ASR CTC 300M](https://github.com/facebookresearch/omnilingual-asr), int8 | Arabic | Apache-2.0 |
| `tts-v1` | [Supertone Supertonic 3](https://huggingface.co/Supertone/supertonic-3), int8 (voice pack) | English, Arabic, Hindi, Spanish, French, Russian, Portuguese, Indonesian, German, Japanese, Turkish, Vietnamese, Korean, Italian (one shared pack) | BigScience Open RAIL-M |

The licence texts are in [LICENSE](LICENSE).
The sources and the changes are in [NOTICE](NOTICE).

We never replace a file in a published release.
A new build gets a new release tag.

## Release `indic-v1`

Each pack is two files: the model and its token table.
The app pins the size and the SHA-256 of each file.
A file with a different SHA-256 fails verification and is not installed.

| Language | File | Size (bytes) | SHA-256 |
|---|---|---|---|
| Bengali | `indicconformer-bn-int8-matmul.onnx` | 186479075 | `f007bc1b12ba8c1eca4823d602e53476e8bf4933fbf989483be6fa7306606149` |
| Bengali | `indicconformer-bn-tokens.txt` | 2727 | `0d9b40979a36302b1e673fe45d90c81bc3f0a82882988b32ae004b866e30290a` |
| Marathi | `indicconformer-mr-int8-matmul.onnx` | 186479075 | `2f175ac95ba94df5e70fb24a59619c4f0c892f62ac6dca353268f3d596a89289` |
| Marathi | `indicconformer-mr-tokens.txt` | 2721 | `1fa2b1c57360011169a4c47d987b41662ab1187c11fcbfa2af2bd827560b2f65` |
| Tamil | `indicconformer-ta-int8-matmul.onnx` | 186479075 | `4cc19473ccb7a23b53f65ba3dad9f3c4e53194551f695d080d10c9e39f13ef94` |
| Tamil | `indicconformer-ta-tokens.txt` | 3042 | `f5a7955261f7eb7156521eecaf17a89521a507d2b740a3a60a2e6f6fb9b3ad36` |
| Telugu | `indicconformer-te-int8-matmul.onnx` | 186479075 | `b99c4ce95c572a50d4f484a88cc7e8b502e2f2ba3d725c5a1937f12ff8536819` |
| Telugu | `indicconformer-te-tokens.txt` | 2706 | `dc0b7bbb9eca8d3c8aebf1eb86e3144c90d692b4008cf5a32d46437b942b23ac` |
| Urdu | `indicconformer-ur-int8-matmul.onnx` | 186479075 | `6f127f2e42680b287b16ddbc67e94454283d0d3909f9a565d9d5bc10900c76b2` |
| Urdu | `indicconformer-ur-tokens.txt` | 2066 | `9ff846180aaf3aa1821dd04a4eaee49c08660d83a56a3760e4ae7a08f0ae50dd` |

Download URL: `https://github.com/saeedtechshop/transly-speech-packs/releases/download/indic-v1/<file>`.

## Release `general-v1`

Two packs:

- The multilingual pack: Whisper small, int8 (encoder, decoder and token table).
  It recognizes Chinese (Mandarin), Korean, Russian, Portuguese, Indonesian, Turkish and Vietnamese.
- The Arabic pack: Omnilingual ASR CTC 300M, int8 (model and token table).

| Pack | File | Size (bytes) | SHA-256 |
|---|---|---|---|
| Multilingual | `whisper-small-encoder.int8.onnx` | 112442483 | `4cbe7b22fa9026b843b60a68640c747de05bafb1a11b57edc0e66c232d9f33a9` |
| Multilingual | `whisper-small-decoder.int8.onnx` | 262226114 | `acad50b5c782696e91b55914cc5ab4f756f1532f76e22aa6fc615f39fb69a8ee` |
| Multilingual | `whisper-small-tokens.txt` | 826781 | `7fd26d1c1c685befd2eb9dc14b7d8187661bbc19ea492f440b3a0f5d88376874` |
| Arabic | `omnilingual-ctc-300m-int8.onnx` | 365352120 | `e7c4e54ee4c4c47829cc6667d5d00ed8ea7bef1dcfeef0fce766f77752a2726c` |
| Arabic | `omnilingual-ctc-300m-tokens.txt` | 86423 | `a7a044c52cb29cbe8b0dc1953e92cefd4ca16b0ed968177b6beab21f9a7d0b31` |

Download URL: `https://github.com/saeedtechshop/transly-speech-packs/releases/download/general-v1/<file>`.

### Where the files come from

All files except `whisper-small-tokens.txt` are the sherpa-onnx int8 exports, without a change:

| File | Source on Hugging Face (pinned commit) |
|---|---|
| `whisper-small-encoder.int8.onnx` | [`csukuangfj/sherpa-onnx-whisper-small`](https://huggingface.co/csukuangfj/sherpa-onnx-whisper-small) @ `8f3c18b358db4d1f2fc1eae49d75cd20989e4309`, `small-encoder.int8.onnx` |
| `whisper-small-decoder.int8.onnx` | the same commit, `small-decoder.int8.onnx` |
| `omnilingual-ctc-300m-int8.onnx` | [`csukuangfj/sherpa-onnx-omnilingual-asr-1600-languages-300M-ctc-int8-2025-11-12`](https://huggingface.co/csukuangfj/sherpa-onnx-omnilingual-asr-1600-languages-300M-ctc-int8-2025-11-12) @ `6abf1ece20cd2308bdb7d13cd78ec1c44fa4c094`, `model.int8.onnx` |
| `omnilingual-ctc-300m-tokens.txt` | the same commit, `tokens.txt` |

The SHA-256 of each copied file matches the LFS object id on Hugging Face.

`whisper-small-tokens.txt` is `small-tokens.txt` of the same Whisper export, with one change.
Whisper uses byte-level BPE, so one token can hold only part of a multi-byte character.
sherpa-onnx 1.13.8 removes invalid UTF-8 from each token before it joins the tokens.
Thus most Arabic, Chinese, Korean and Vietnamese characters disappear from the text ([k2-fsa/sherpa-onnx#3965](https://github.com/k2-fsa/sherpa-onnx/pull/3965), fixed after 1.13.8).
In the changed table, each token that is not valid UTF-8 on its own stores every non-ASCII byte b as the character U+F700 + b.
1476 of the 50257 tokens have this change.
The app turns these characters back into bytes after the decode.
The script that makes the file is `scripts/speech-packs/escape_whisper_tokens.py` in the Transly app repository.

### Use the Whisper pack with sherpa-onnx

```python
import sherpa_onnx, soundfile as sf

recognizer = sherpa_onnx.OfflineRecognizer.from_whisper(
    encoder="whisper-small-encoder.int8.onnx",
    decoder="whisper-small-decoder.int8.onnx",
    tokens="whisper-small-tokens.txt",
    language="ko",
    tail_paddings=300,
    num_threads=4,
)
audio, rate = sf.read("speech.wav", dtype="float32")  # 16 kHz, mono
stream = recognizer.create_stream()
stream.accept_waveform(rate, audio)
recognizer.decode_stream(stream)
text = stream.result.text
# Join the escaped bytes again (U+F780 to U+F7FF).
raw = b"".join(
    bytes([ord(c) - 0xF700]) if "\uf780" <= c <= "\uf7ff" else c.encode()
    for c in text
)
print(raw.decode("utf-8", errors="ignore"))
```

The Omnilingual pack loads with `sherpa_onnx.OfflineRecognizer.from_omnilingual_asr_ctc(model=..., tokens=...)`.
It needs no language: the model finds the language itself.

### Accuracy

Test set: the first 100 distinct sentences of the FLEURS test split of each language, one recording each.
Decoder: sherpa-onnx 1.13.8 on a Mac CPU, 2 threads, Whisper with 300 tail padding frames.
Scoring: `jiwer`, after NFC normalization, lowercase, and removal of punctuation.
Turkish uses Turkish casing (I and İ).
Arabic is compared without diacritics, with one form of alef.
For Chinese and Korean, the score is the character error rate without spaces.
Chinese output is converted to Simplified characters first, as the app does.

| Language | FLEURS | Pack | WER % | CER % |
|---|---|---|---|---|
| Arabic | `ar_eg` | Arabic (Omnilingual) | 19.9 | 7.6 |
| Russian | `ru_ru` | Multilingual (Whisper) | 11.4 | 3.2 |
| Portuguese | `pt_br` | Multilingual (Whisper) | 7.8 | 3.2 |
| Indonesian | `id_id` | Multilingual (Whisper) | 19.2 | 5.4 |
| Turkish | `tr_tr` | Multilingual (Whisper) | 16.6 | 4.4 |
| Vietnamese | `vi_vn` | Multilingual (Whisper) | 23.4 | 11.4 |
| Chinese (Mandarin) | `cmn_hans_cn` | Multilingual (Whisper) | n/a | 13.1 |
| Korean | `ko_kr` | Multilingual (Whisper) | 21.2 | 7.2 |

The comparison with the other candidates is in `docs/speech-pack-evaluation.md` of the Transly app repository.

## Release `tts-v1`

One voice pack: Supertonic 3 by Supertone, int8.
It reads 14 languages aloud: en, ar, hi, es, fr, ru, pt, id, de, ja, tr, vi, ko and it.
It reads text as characters, so it needs no phonemizer and no espeak-ng.

| File | Size (bytes) | SHA-256 |
|---|---|---|
| `supertonic-3-duration_predictor.int8.onnx` | 3700147 | `c3eb91414d5ff8a7a239b7fe9e34e7e2bf8a8140d8375ffb14718b1c639325db` |
| `supertonic-3-text_encoder.int8.onnx` | 36416150 | `c7befd5ea8c3119769e8a6c1486c4edc6a3bc8365c67621c881bbb774b9902ff` |
| `supertonic-3-vector_estimator.int8.onnx` | 78400833 | `20cd86fa5c6effedfda0e7cffe5b0569ca401c440a0c3a1d72bf39286c0db3fd` |
| `supertonic-3-vocoder.int8.onnx` | 25991073 | `e923d60f53f95eb1ce235f1dc33ec56d9c057823c96fa6f8acf98f32b0da6152` |
| `supertonic-3-tts.json` | 8253 | `42078d3aef1cd43ab43021f3c54f47d2d75ceb4e75f627f118890128b06a0d09` |
| `supertonic-3-unicode_indexer.bin` | 262144 | `8402ca48e5189a8950138580b0fff64db6f072f24ac07cd54ba8b2fbb9883b30` |
| `supertonic-3-voice.bin` | 517168 | `67d5209b0ee8ce6c74105ffbe12fe6a7628aea3b4ba2fcb308a4a67938a93ce8` |
| `supertonic-3-LICENSE.txt` | 15007 | `0d944a9110fed9a9602d60e0423a272903e7bd21ab060490774efc77c2275e9f` |

Download URL: `https://github.com/saeedtechshop/transly-speech-packs/releases/download/tts-v1/<file>`.

The seven model files are the sherpa-onnx int8 export ([`sherpa-onnx-supertonic-3-tts-int8-2026-05-11.tar.bz2`](https://github.com/k2-fsa/sherpa-onnx/releases/download/tts-models/sherpa-onnx-supertonic-3-tts-int8-2026-05-11.tar.bz2)), without a change.
Only the file names have the prefix `supertonic-3-`.
`voice.bin` holds ten preset voices: speakers 0 to 4 are F1 to F5, and speakers 5 to 9 are M1 to M5.
The Transly app uses speaker 5 (M1).

The model licence is BigScience Open RAIL-M ([LICENSE](LICENSE), part 4).
It permits commercial use, but paragraph 5 and Attachment A restrict how the model and its output can be used.
If you give the model to other people, you must give them the licence and pass these restrictions on to them.

```python
import sherpa_onnx, soundfile as sf

tts = sherpa_onnx.OfflineTts(sherpa_onnx.OfflineTtsConfig(
    model=sherpa_onnx.OfflineTtsModelConfig(
        supertonic=sherpa_onnx.OfflineTtsSupertonicModelConfig(
            duration_predictor="supertonic-3-duration_predictor.int8.onnx",
            text_encoder="supertonic-3-text_encoder.int8.onnx",
            vector_estimator="supertonic-3-vector_estimator.int8.onnx",
            vocoder="supertonic-3-vocoder.int8.onnx",
            tts_json="supertonic-3-tts.json",
            unicode_indexer="supertonic-3-unicode_indexer.bin",
            voice_style="supertonic-3-voice.bin",
        ),
        num_threads=4,
    ),
))
config = sherpa_onnx.GenerationConfig()
config.sid = 5
config.num_steps = 5
config.extra["lang"] = "ar"
audio = tts.generate("كم سعر هذه الحقيبة؟", config)
sf.write("speech.wav", audio.samples, audio.sample_rate)
```

## Use the IndicConformer packs with sherpa-onnx

```python
import sherpa_onnx, soundfile as sf

recognizer = sherpa_onnx.OfflineRecognizer.from_nemo_ctc(
    model="indicconformer-bn-int8-matmul.onnx",
    tokens="indicconformer-bn-tokens.txt",
    num_threads=4,
)
audio, rate = sf.read("speech.wav", dtype="float32")  # 16 kHz, mono
stream = recognizer.create_stream()
stream.accept_waveform(rate, audio)
recognizer.decode_stream(stream)
print(stream.result.text)
```

The models give text without punctuation.
They are not streaming models: decode one utterance at a time.

## IndicConformer accuracy

Test set: the first 100 distinct sentences of the FLEURS test split of each language, one recording each.
Scoring: WER and CER with `jiwer`, after NFC normalization, lowercase, and removal of punctuation and zero-width joiners.
The references keep their digits, but the models write numbers as words, so each number counts as an error.
Decoder: sherpa-onnx 1.13.8 on a Mac CPU, 2 threads.

| Language | Audio (min) | WER % | CER % |
|---|---|---|---|
| Bengali | 23.6 | 17.5 | 4.7 |
| Marathi | 23.6 | 18.5 | 5.2 |
| Tamil | 22.3 | 26.7 | 10.6 |
| Telugu | 19.3 | 24.8 | 8.9 |
| Urdu | 16.2 | 16.8 | 6.4 |

The unquantized models give the same WER within 0.3 points.

## How the IndicConformer files are made

The source is the official AI4Bharat checkpoint of each language.
The checkpoints come from the AI4Bharat object store, `https://objectstore.e2enetworks.net/indicconformer/models/indicconformer_stt_<lang>_hybrid_rnnt_large.nemo`.
The same files are on Hugging Face (`ai4bharat/indicconformer_stt_<lang>_hybrid_ctc_rnnt_large`), behind a licence gate.

| Language | Checkpoint SHA-256 (523192320 bytes each) |
|---|---|
| Bengali | `78b724bce03aa1f986a7217176e82e8088be25c0e786801cced61ca11124ddc7` |
| Marathi | `f7c161ab5bd4a9bb62c5ba33c2667c00f51db8497eb0baf4bb89fa2388758979` |
| Tamil | `40a6fce3374ceb67daa2a071e92f0948c51c6a95b734d4881a3bcfe0b1bed32a` |
| Telugu | `70aea39388043beae2b06af21df47811d23a743ac5641d7b5c0d20d04fa87a3c` |
| Urdu | `c4857939999237ac5165f28d5650be6f84a75bbb31c05b280fab030d0d97e1d2` |

The export script is in the Transly app repository (`scripts/speech-packs/export_indicconformer.py`).
It does these steps:

1. It checks the SHA-256 of the checkpoint.
2. It loads the checkpoint with the [AI4Bharat fork of NeMo](https://github.com/AI4Bharat/NeMo), branch `nemo-v2`, commit `8dce88cf8e94963e2033c3137f7b9993b51db88a`.
3. It keeps the CTC decoder and cuts its output layer down to the 256 tokens of the language plus the blank token.
   The original model uses the same outputs when it decodes with `language_id`.
   The script compares the two on a fixed input. For all five languages, the difference is zero.
4. It exports the model to ONNX, opset 17, and compares the ONNX output with the PyTorch output.
5. It adds the metadata that the sherpa-onnx NeMo CTC loader reads: `vocab_size=257`, `normalize_type=per_feature`, `subsampling_factor=4`, `model_type=EncDecCTCModelBPE`.
6. It quantizes the model with ONNX Runtime: `quantize_dynamic(op_types_to_quantize=['MatMul'], per_channel=True, weight_type=QuantType.QInt8)`.
7. It writes the token table: the 256 tokens of the language with ids 0 to 255, then `<blk>` with id 256.

Environment: Python 3.11, `torch==2.4.1`, `numpy==1.26.4`, `onnx==1.16.2`, `onnxruntime==1.19.2`.
Install the NeMo fork with `--no-deps`: its own requirements ask for `triton`, which does not exist for macOS.

The export is deterministic.
With this environment, two runs give files with the same SHA-256.

## Licence

Each release keeps the licence of its source model:

- `indic-v1`: AI4Bharat IndicConformer, MIT licence.
- `general-v1`, Whisper files: OpenAI Whisper, MIT licence.
- `general-v1`, Omnilingual files: Meta Omnilingual ASR, Apache License 2.0.

The texts are in [LICENSE](LICENSE).
The changes for Transly are under the licence of the file that they change ([NOTICE](NOTICE)).
