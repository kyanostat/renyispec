# renyispec

**Robust power spectral density (PSD) estimation with the spectral Rényi divergence**


**スペクトル Rényi ダイバージェンスによるロバストなパワースペクトル推定**

[English](#english) | [日本語](#日本語)

[Fitted spectra](figures/fit_comparison.png)

---

## English

### Overview

Time series often contain periodic signals (e.g., annual and semi-annual terms) on top of a broadband background component. 
When a parametric spectral model is fitted to the periodogram with the standard Whittle / Itakura–Saito (IS) objective, 
these spectral peaks can bias the estimated background spectrum.

This repository provides a Jupyter notebook demonstrating **robust spectral estimation based on the spectral Rényi divergence**. 
With a Rényi hyper-parameter $0<\alpha<1$, the background spectrum is recovered even when peaks are present.

### What the notebook does

1. **Model** — a two-component (low- and high-frequency) Brune-type one-sided PSD:

   $$S(f;\theta)=\frac{s_{\mathrm{low}}^2}{[1+(|f|/f_{c,\mathrm{low}})^{p_{\mathrm{low}}}]^2}+\frac{s_{\mathrm{high}}^2}{[1+(|f|/f_{c,\mathrm{high}})^{p_{\mathrm{high}}}]^2}$$

2. **Simulation** — Gaussian data with this PSD, with and without added seasonal sinusoids.
3. **Estimation** — fits the model to the periodogram by minimizing either
   - the spectral Rényi divergence $L_\alpha(I,S)$ (default $\alpha=0.1$), or
   - the Itakura–Saito divergence (for comparison),

   using L-BFGS-B over a grid of fixed corner frequencies.
4. **Comparison** — fitted spectra, individual components, parameter table and log-RMSE against the ground truth.
5. **Uncertainty** — parametric bootstrap with pointwise 95% bands, including a scale-bias correction for the Rényi estimator (`renyi_scale_factor`).

### Requirements

- Python ≥ 3.10 (tested with 3.12)
- NumPy, SciPy, pandas, Matplotlib, Jupyter

```bash
pip install numpy scipy pandas matplotlib jupyter
```

### Quick start

```bash
git clone https://github.com/<your-account>/renyispec.git
cd renyispec
jupyter notebook notebooks/RobustSpectralEstimation_renyi.ipynb
```

Run all cells from top to bottom. Key settings are in Section 1:

| Variable | Default | Meaning |
|:--|:--|:--|
| `RENYI_ALPHA` | `0.1` | Rényi parameter, $0<\alpha<1$ |
| `SEASONAL_SCALE` | `1.0` | Strength of the periodic components (`0` disables them) |
| `FC_LOW_GRID`, `FC_HIGH_GRID` | `(1e-4, 1e-3, 1e-2)`, `(0.05, 0.5)` | Candidate corner frequencies |
| `BOOTSTRAP_SIZE` | `40` | Number of bootstrap replicates (increase for stable tail quantiles) |

Tip: set `SEASONAL_SCALE = 0` to confirm that both series, PSDs and fits become identical.

### Repository structure

```
renyi-spectral-fit/
├── notebooks/
│   └── RobustSpectralEstimation_renyi.ipynb
├── figures/            # figures for the README (optional)
├── LICENSE
└── README.md
```

### References

- Takabatake & Yano (2026). On robustness of spectral Rényi divergence. *Annals of the Institute of Statistical Mathematics*. https://doi.org/10.1007/s10463-026-00983-y
- Kano et al. (2025). Spatio-temporal characteristics in the GEONET F5 solution in the frequency domain estimated based on the robust spectral analysis. *Earth, Planets and Space*. https://doi.org/10.1186/s40623-025-02236-3

### Citation


### License


---

## 日本語

### 概要

時系列には、広帯域の背景時系列に加えて、強い周期成分が含まれることがあります。
通常の Whittle 尤度（Itakura–Saito ダイバージェンス）でピリオドグラムにスペクトルモデルを当てはめると、こうしたスペクトルピークに引きずられて背景スペクトルの推定にバイアスが生じます。

このリポジトリは、**スペクトル Rényi ダイバージェンス**によるロバストなスペクトル推定をデモする Jupyter ノートブックです。
$0<\alpha<1$ の Rényi パラメータにより、ピークがあっても背景スペクトルを頑健に推定できます。

### ノートブックの内容

1. **モデル**：低周波・高周波の2成分からなる Brune 型の片側PSD（式は英語版を参照）
2. **シミュレーション**：このPSDをもつガウス時系列を生成し、周期成分を加えた系列と比較
3. **推定**：Rényi ダイバージェンス（既定 $\alpha=0.1$）と IS ダイバージェンスのそれぞれを、コーナー周波数のグリッド上で L-BFGS-B により最小化
4. **比較**：推定スペクトル、成分ごとの曲線、パラメータ表、真値に対する log-RMSE
5. **不確実性評価**：パラメトリックブートストラップによる各周波数の95%区間（Rényi 推定のスケールバイアスを `renyi_scale_factor` で補正）

### 必要環境

- Python 3.10 以上（3.12 で動作確認）
- NumPy, SciPy, pandas, Matplotlib, Jupyter

### 使い方

上記「Quick start」の手順でノートブックを開き、上から順に全セルを実行してください。主な設定はセクション1にあります（表は英語版を参照）。`SEASONAL_SCALE = 0` にすると周期成分がなくなり、2つの推定結果が一致することを確認できます。

### 参考文献・引用


### ライセンス
