# Finding North Pole star (3200 BCE – 2026 CE)

Finds the naked-eye star nearest the north celestial pole at each epoch from 3200 BCE to 2026 CE, as the pole moves under precession of the equinoxes. Star positions come from the Swiss Ephemeris and are computed *of date*, so precession, nutation and proper motion are all included.

Everything is in `polestar_finder.ipynb`: point it at the ephemeris data (below) and run the cells top to bottom.

## Method

A star's ecliptic latitude `β_S` is effectively fixed over this span. The celestial pole lies at angular distance `ε` from the ecliptic pole, where `ε` is the obliquity of the ecliptic, so the pole sits at ecliptic latitude `β_P = 90° − ε` and longitude `λ_P = 90°`. Converting the star to the equator of date gives its pole distance:

```
p_S(T) = 90° − δ_S(T),    sin δ_S = sin β_S cos ε + cos β_S sin ε sin λ_S
```

Precession advances `λ_S`, and `p_S` is smallest at `λ_S = 90°`, the closest the star ever comes to the pole:

```
p_min(S) = |β_S − β_P|
```

So a star can come within a tolerance `τ` of the pole only if `|β_S − β_P| ≤ τ`.

At each epoch the pole star is the brightest naked-eye star (`m_S ≤ m_lim`) nearest the pole:

```
S = argmin p_S(T),    ties broken by smaller m_S
```

It counts as a pole star when `p_S(T) ≤ τ`. When no star lies within `τ` the epoch is a gap, with no true pole star, and the nearest star is reported for reference.

Two practical points. The declination is computed analytically from the ecliptic position and the obliquity, so each star takes one ephemeris call per epoch instead of two. The search is restricted to stars with ecliptic latitude between 50° and 83° (`β_P` is about 66.6°); since `p_S(T) ≥ |β_S − β_P|`, that window always contains the nearest star and reproduces a full-catalogue search.

## Parameters

Set at the top of the notebook.

- `TAU = 5.0` — the largest separation at which a star is still treated as a pole marker. This is the loose end of usage in the literature; Vega and Deneb are described as pole stars at about 5°.
- `TAU_STRICT = 1.0` — the strict sense, within which Polaris appears fixed to the unaided eye. Epochs meeting it are flagged.
- `M_LIM = 4.0` — the magnitude gate. It keeps Thuban (3.68) and Polaris (2.02) but drops fainter stars that, while visible to the naked eye (detection is near magnitude 6), are too dim to act as markers. It is a practical cutoff set just above Thuban, the faintest star generally recognised as a former pole star, not a physical constant.

## Requirements

```
pip install pyswisseph
```

plus the Swiss Ephemeris data: the fixed-star catalogue `sefstars.txt` and the DE431 long-range files.

The DE431 files are needed because the start epoch, 3200 BCE (JD ≈ 552805), is below the floor of the built-in Moshier ephemeris (near 3000 BCE). If you only need 3000 BCE onward, set `FLG_EC = swe.FLG_MOSEPH` in the setup cell and skip the DE431 files entirely; Moshier needs only `sefstars.txt`.

## Data files

Put `sefstars.txt` and these twenty `.se1` files in one directory and point `EPHE_PATH` at it. The default is an `ephe/` folder next to the notebook.

```
seplm36 seplm30 seplm24 seplm18 seplm12 seplm06 sepl_00 sepl_06 sepl_12 sepl_18
semom36 semom30 semom24 semom18 semom12 semom06 semo_00 semo_06 semo_12 semo_18
```

They are in the `ephe/` folder of the Swiss Ephemeris repository, <https://github.com/aloistr/swisseph/tree/master/ephe>, about 0.5 MB each. The `sepl*` files hold planet positions and `semo*` the Moon; the `m36 … m06` blocks cover the BCE range in 600-year steps and `_00 … _18` the CE range.

Download them into `ephe/`:

bash / git-bash:

```bash
mkdir -p ephe && cd ephe
base="https://raw.githubusercontent.com/aloistr/swisseph/master/ephe"
for f in seplm36 seplm30 seplm24 seplm18 seplm12 seplm06 sepl_00 sepl_06 sepl_12 sepl_18 \
         semom36 semom30 semom24 semom18 semom12 semom06 semo_00 semo_06 semo_12 semo_18; do
    curl -fsSL -O "$base/$f.se1"
done
curl -fsSL -O "$base/sefstars.txt"
```

PowerShell:

```powershell
New-Item -ItemType Directory -Force ephe | Out-Null; Set-Location ephe
$base = "https://raw.githubusercontent.com/aloistr/swisseph/master/ephe"
$se1 = "seplm36","seplm30","seplm24","seplm18","seplm12","seplm06","sepl_00","sepl_06","sepl_12","sepl_18",
       "semom36","semom30","semom24","semom18","semom12","semom06","semo_00","semo_06","semo_12","semo_18"
foreach ($f in $se1) { Invoke-WebRequest "$base/$f.se1" -OutFile "$f.se1" }
Invoke-WebRequest "$base/sefstars.txt" -OutFile "sefstars.txt"
```

## Running

Set `EPHE_PATH` in the setup cell to the directory with the data files; the default `"ephe"` works when Jupyter is launched from the folder that contains `ephe/`. Run the cells top to bottom. The pre-filter cell prints the working-set size (about 146 stars), which confirms the catalogue and the `.se1` files are being read; the final cell prints the table.

## Output

```
North pole star   (tau = 5 deg,  m_lim = 4.0)

     year | star                |   p_S(T) |   mag | status
--------------------------------------------------------------------------
 3200 BCE | Thuban (α Dra)      |   2.28 d |  3.68 | pole star
 3000 BCE | Thuban (α Dra)      |   1.15 d |  3.68 | pole star
 2750 BCE | Thuban (α Dra)      |   0.28 d |  3.68 | strict pole star (<= 1 deg)
 2500 BCE | Thuban (α Dra)      |   1.68 d |  3.68 | pole star
 2250 BCE | Thuban (α Dra)      |   3.09 d |  3.68 | pole star
 2000 BCE | Thuban (α Dra)      |   4.49 d |  3.68 | pole star
 1750 BCE | Ketu (κ Dra)        |   5.41 d |  3.89 | gap: no pole star (nearest shown)
 1500 BCE | Ketu (κ Dra)        |   4.83 d |  3.89 | pole star
 1250 BCE | Ketu (κ Dra)        |   4.68 d |  3.89 | pole star
 1000 BCE | Ketu (κ Dra)        |   5.03 d |  3.89 | gap: no pole star (nearest shown)
  750 BCE | Ketu (κ Dra)        |   5.76 d |  3.89 | gap: no pole star (nearest shown)
  500 BCE | Ketu (κ Dra)        |   6.77 d |  3.89 | gap: no pole star (nearest shown)
  250 BCE | Kochab (β UMi)      |   7.61 d |  2.08 | gap: no pole star (nearest shown)
     1 CE | Kochab (β UMi)      |   8.29 d |  2.08 | gap: no pole star (nearest shown)
   251 CE | Kochab (β UMi)      |   9.08 d |  2.08 | gap: no pole star (nearest shown)
   501 CE | Polaris (α UMi)     |   8.99 d |  2.02 | gap: no pole star (nearest shown)
   751 CE | Polaris (α UMi)     |   7.60 d |  2.02 | gap: no pole star (nearest shown)
  1001 CE | Polaris (α UMi)     |   6.21 d |  2.02 | gap: no pole star (nearest shown)
  1251 CE | Polaris (α UMi)     |   4.81 d |  2.02 | pole star
  1501 CE | Polaris (α UMi)     |   3.41 d |  2.02 | pole star
  1751 CE | Polaris (α UMi)     |   2.03 d |  2.02 | pole star
  2001 CE | Polaris (α UMi)     |   0.74 d |  2.02 | strict pole star (<= 1 deg)
  2026 CE | Polaris (α UMi)     |   0.63 d |  2.02 | strict pole star (<= 1 deg)
```

Thuban (α Dra) holds the pole from before 3200 BCE, closest at about 0.28° near 2750 BCE. A gap follows, in which the nearest naked-eye stars (κ Draconis, then Kochab, β UMi) stay 5–9° from the pole, so by the strict reading there is no pole star then. Polaris (α UMi) closes in through the first millennium CE, becomes a pole star around 1250 CE, and reaches 0.63° in 2026.

The grid is sampled every 250 years (with an extra row at 3200 BCE and 2026 CE), so the reign boundaries and exact closest-approach years fall between rows; change `years` in the last cell for finer steps.

## Implementation

- Positions are of date via `swe.fixstar2_ut` in `FLG_SWIEPH` mode; the obliquity is the true obliquity of date.
- The declination is derived from the ecliptic longitude and latitude with the obliquity, matching the Swiss equatorial output to ~0.0001″, so only one ephemeris call per star is needed.
- Stars with no traditional name are queried by Bayer designation prefixed with a comma (e.g. `,kaCep`). Swiss Ephemeris matches the Bayer field only that way; without the comma about 40% of the catalogue silently fails to match.
- Output names carry the official Bayer designation in brackets, e.g. Thuban (α Dra).

## Data

The Swiss Ephemeris data files are not bundled here. Download them from the upstream repository as above. Swiss Ephemeris is licensed separately (AGPL, or a commercial licence): see <https://www.astro.com/swisseph/>. 
