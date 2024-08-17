---
title: "Kubernetes Memory Units"
date: 2020-09-13T09:19:42+01:00
draft: false

tags: [Kubernetes]
categories: [Software Development]
---

Default memory unit in Kubernetes is bytes, in addition to other shorter forms with one of supported suffixes: E, P, T, G, M, K, Ei, Pi, Ti, Gi, Mi, Ki. 

For example: `128974848, 129e6, 129M , 123Mi`

Have you ever wondered what do those suffixes mean? 

A popular reference is https://medium.com/@betz.mark/understanding-resource-limits-in-kubernetes-memory-6b41e9a955f9 

But the root definition is at https://en.wikipedia.org/wiki/Binary_prefix

<table class="infobox noprint" style="padding: 0; text-align: left; width: 0">
<tbody><tr>
<th colspan="2" class="navbox-title" style="text-align: center">Prefixes for multiples of <br><a href="/wiki/Bit" title="Bit">bits</a> (bit) or <a href="/wiki/Byte" title="Byte">bytes</a> (B)
</th></tr>
<tr>
<td>
<table style="border: 1px #aaa solid">
<tbody><tr>
<th colspan="4" style="background:lavender; text-align: center"><a href="/wiki/Decimal_prefix" class="mw-redirect" title="Decimal prefix">Decimal</a>
</th></tr>
<tr>
<th colspan="2" style="background: #eeeeff; text-align:center; padding:0.2em 0.3em;">Value
</th>
<th colspan="2" style="background: #eeeeff; text-align: center"><a href="/wiki/SI_prefix" class="mw-redirect" title="SI prefix">SI</a>
</th></tr>
<tr>
<td>1000
</td>
<td>10<sup>3</sup>
</td>
<td>k</td>
<td><a href="/wiki/Kilo-" title="Kilo-">kilo</a>
</td></tr>
<tr>
<td>1000<sup>2</sup>
</td>
<td>10<sup>6</sup>
</td>
<td>M</td>
<td><a href="/wiki/Mega-" title="Mega-">mega</a>
</td></tr>
<tr>
<td>1000<sup>3</sup>
</td>
<td>10<sup>9</sup>
</td>
<td>G</td>
<td><a href="/wiki/Giga-" title="Giga-">giga</a>
</td></tr>
<tr>
<td>1000<sup>4</sup>
</td>
<td>10<sup>12</sup>
</td>
<td>T</td>
<td><a href="/wiki/Tera-" title="Tera-">tera</a>
</td></tr>
<tr>
<td>1000<sup>5</sup>
</td>
<td>10<sup>15</sup>
</td>
<td>P</td>
<td><a href="/wiki/Peta-" title="Peta-">peta</a>
</td></tr>
<tr>
<td>1000<sup>6</sup>
</td>
<td>10<sup>18</sup>
</td>
<td>E</td>
<td><a href="/wiki/Exa-" title="Exa-">exa</a>
</td></tr>
<tr>
<td>1000<sup>7</sup>
</td>
<td>10<sup>21</sup>
</td>
<td>Z</td>
<td><a href="/wiki/Zetta-" title="Zetta-">zetta</a>
</td></tr>
<tr>
<td>1000<sup>8</sup>
</td>
<td>10<sup>24</sup>
</td>
<td>Y</td>
<td><a href="/wiki/Yotta-" title="Yotta-">yotta</a>
</td></tr></tbody></table>
</td>
<td>
<table style="border: 1px #aaa solid">
<tbody><tr>
<th colspan="6" style="background:lavender; text-align:center"><a class="mw-selflink selflink">Binary</a>
</th></tr>
<tr>
<th colspan="2" style="background: #eeeeff; text-align:center;">Value
</th>
<th colspan="2" style="background: #eeeeff; text-align:center"><a href="/wiki/IEC_80000-13" class="mw-redirect" title="IEC 80000-13">IEC</a>
</th>
<th colspan="2" style="background: #eeeeff; text-align:center"><a href="/wiki/JEDEC_memory_standards#Unit_prefixes_for_semiconductor_storage_capacity" title="JEDEC memory standards">JEDEC</a>
</th></tr>
<tr>
<td>1024
</td>
<td>2<sup>10</sup>
</td>
<td>Ki</td>
<td><a href="/wiki/Kibi-" class="mw-redirect" title="Kibi-">kibi</a>
</td>
<td>K</td>
<td>kilo
</td></tr>
<tr>
<td>1024<sup>2</sup>
</td>
<td>2<sup>20</sup>
</td>
<td>Mi</td>
<td><a href="/wiki/Mebi-" class="mw-redirect" title="Mebi-">mebi</a>
</td>
<td>M</td>
<td>mega
</td></tr>
<tr>
<td>1024<sup>3</sup>
</td>
<td>2<sup>30</sup>
</td>
<td>Gi</td>
<td><a href="/wiki/Gibi-" class="mw-redirect" title="Gibi-">gibi</a>
</td>
<td>G</td>
<td>giga
</td></tr>
<tr>
<td>1024<sup>4</sup>
</td>
<td>2<sup>40</sup>
</td>
<td>Ti</td>
<td><a href="/wiki/Tebi-" class="mw-redirect" title="Tebi-">tebi</a>
</td>
<td style="text-align:center">–</td>
<td>
</td></tr>
<tr>
<td>1024<sup>5</sup>
</td>
<td>2<sup>50</sup>
</td>
<td>Pi</td>
<td><a href="/wiki/Pebi-" class="mw-redirect" title="Pebi-">pebi</a>
</td>
<td style="text-align:center">–</td>
<td>
</td></tr>
<tr>
<td>1024<sup>6</sup>
</td>
<td>2<sup>60</sup>
</td>
<td>Ei</td>
<td><a href="/wiki/Exbi-" class="mw-redirect" title="Exbi-">exbi</a>
</td>
<td style="text-align:center">–</td>
<td>
</td></tr>
<tr>
<td>1024<sup>7</sup>
</td>
<td>2<sup>70</sup>
</td>
<td>Zi</td>
<td><a href="/wiki/Zebi-" class="mw-redirect" title="Zebi-">zebi</a>
</td>
<td style="text-align:center">–</td>
<td>
</td></tr>
<tr>
<td>1024<sup>8</sup>
</td>
<td>2<sup>80</sup>
</td>
<td>Yi</td>
<td><a href="/wiki/Yobi-" class="mw-redirect" title="Yobi-">yobi</a>
</td>
<td style="text-align:center">–</td>
<td>
</td></tr></tbody></table>
</td></tr>
</tbody></table>