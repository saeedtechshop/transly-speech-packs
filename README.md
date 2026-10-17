# Transly speech packs

This repository hosts the on-device speech recognition models that the Transly app downloads.
The app uses a speech pack when the phone's own recognizer cannot recognize a language offline.
Each pack runs on [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx) as a NeMo CTC model, on the phone CPU.

The models are modified versions of [AI4Bharat IndicConformer](https://github.com/AI4Bharat/IndicConformerASR) (MIT licence).
The changes are in [NOTICE](NOTICE).

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

We never replace a file in a published release.
A new build gets a new release tag.

## Use with sherpa-onnx

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

## Accuracy

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

## How the files are made

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

The models are by AI4Bharat, under the MIT licence ([LICENSE](LICENSE)).
The changes for Transly are under the same licence ([NOTICE](NOTICE)).
