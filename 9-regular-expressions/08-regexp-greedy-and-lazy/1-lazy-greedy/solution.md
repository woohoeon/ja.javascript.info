
結果は: `match:123 4` です。

まず、怠惰 `pattern:\d+?` はできるだけ小さい桁を取ろうとしますが、スペースまで到達する必要があるので、 `match:123` となります。

次に、2つ目の `\d+?` は1桁だけを取ります。なぜならそれで十分だからです。