
<p>
L'objectif est double : faire fonctionner pocketsphinx en Français et sur un vocabulaire réduit (tout au plus quelques commandes)  pour des raisons de performance et précision.
</p>

<p>
Tout est testé sur une Mint 19.2 mais il ne devrait pas y avoir d'obstacle à faire tourner tout cela sur une autre distribution.
</p>

<h1>1</h1>

<p>
download dict, hmm et lm sur :<br>
<a href="https://sourceforge.net/projects/cmusphinx/files/Acoustic and Language Models/French/" title="https://sourceforge.net/projects/cmusphinx/files/Acoustic and Language Models/French/" class="https">https://sourceforge.net/projects/cmusphinx/files/Acoustic and Language Models/French/</a><br>
soit :<br>
fr.dict<br>
cmusphinx-fr-ptm-52<br>
et<br>
fr-small.lm.gz<br>
Copier et décompresser dans dossier arbitraire
</p>

<h1>2</h1>

<p>
récupérer les sources de srilm<br>
<a href="http://www.speech.sri.com/projects/srilm/download.html" title="http://www.speech.sri.com/projects/srilm/download.html" class="http">http://www.speech.sri.com/projects/srilm/download.html</a><br>
Pas de difficulté pour compiler. Juste adapter le Makefile en renseignant SRILM avec le chemin vers le dossier contenant les sources, puis :
</p>
<div class="zim-object">
<pre><code class="sh">make World</code></pre>
</div>
<p>
 Les exécutables seront sous bin/i686-m64 (en fonction de l'architecture)
</p>

<h1>3</h1>

<p>
copier le vocabulaire désiré dans un fichier texte, exemple : words.txt
</p>
<div class="zim-object">
<pre><code class="c">un
deux
trois</code></pre>
</div>

<p>
Puis construire un lm sur mesure pour ce vocabulaire en expurgeant l'original des infos inutiles :
</p>
<div class="zim-object">
<pre><code class="sh">ngram -vocab words.txt -limit-vocab -lm french-small.lm -write-lm small.lm</code></pre>
</div>

<br>

<h1>4</h1>

<p>
installer et lancer pocketsphinx
</p><h1>6</h1>

<p>
après installation et start de pulseaudio sur raspberry 3 buster
</p>
<div class="zim-object">
<pre><code class="sh">aptitude install pulseaudio
pulseaudio --start
aptitude install pocketsphinx
pocketsphinx_continuous -dict fr.dict -hmm cmusphinx-fr-ptm-5.2/ -lm small.lm -inmic yes</code></pre>
</div>

<p>
Un mot parmi un vocabulaire de taille 10 détecté en environ 2 secondes (un coeur chargé sporadiquement à 20%)
</p>

<br>
<br>
<br>


	</div>

	<br />

	<div class='page-footer'>

	</div>

	

</div>
<div class="zim-object">
<pre><code class="sh">aptitude install pocketsphinx
pocketsphinx_continuous -dict fr.dict -hmm cmusphinx-fr-ptm-5.2/ -lm small.lm -inmic yes</code></pre>
</div>

<br>

<h1>5</h1>

<p>
pour jouer avec python :
</p>
<div class="zim-object">
<pre><code class="sh">aptitude install swig
aptitude install python3-dev
aptitude install libpulse-dev
aptitude install libasound2-dev
pip3 install pocketsphinx</code></pre>
</div>

<p>
Lancer ces quelques lignes (<a href="https://pypi.org/project/pocketsphinx/" title="https://pypi.org/project/pocketsphinx/" class="https">https://pypi.org/project/pocketsphinx/</a>)
</p>
<div class="zim-object">
<pre><code class="python">import os
from pocketsphinx import LiveSpeech
speech = LiveSpeech(
    verbose=False,
    sampling_rate=16000,
    buffer_size=2048,
    no_search=False,
    full_utt=False,
    hmm='cmusphinx-fr-ptm-5.2/',
    lm='small.lm',
    dic='fr.dict'
)

for phrase in speech:
    print(phrase)</code></pre>
</div>

<br>

<h1>6</h1>

<p>
après installation et start de pulseaudio sur raspberry 3 buster
</p>
<div class="zim-object">
<pre><code class="sh">aptitude install pulseaudio
pulseaudio --start
aptitude install pocketsphinx
pocketsphinx_continuous -dict fr.dict -hmm cmusphinx-fr-ptm-5.2/ -lm small.lm -inmic yes</code></pre>
</div>

<p>
Un mot parmi un vocabulaire de taille 10 détecté en environ 2 secondes (un coeur chargé sporadiquement à 20%)
</p>

<br>
<br>
<br>



