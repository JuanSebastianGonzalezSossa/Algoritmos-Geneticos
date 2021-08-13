//# Algoritmos-Geneticos
//Software de algoritmos genéticos que muta desde una mutación hasta llegar a la cadena de texto deseada en nuestro caso será Cadena="Ingeniería informática, inteligencia artificial". nuestro caso  

<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="utf-8" />
		<title>Genetic Algorithm in JavaScript</title>
	</head>
	<body style="height: 750px;
	background-repeat:repeat-y;
	background-position-x:center;
	background-position-y:center;
	background-size: 84rem;
	background-image: url('https://www.wallpapertip.com/wmimgs/78-783455_artificial-intelligence-wallpaper-4k.jpg');
	background-attachment: scroll;"
	>
		<script type="text/javascript">
			var a = Math;
			var b = 'Ingeniería informática, inteligencia artificial';
			var randomLetter = function () {
				var letters = 'Iacefgilmnortáí , ';
				return letters.charAt(a.floor((a.random()*letters.length)));
			}

			var similarString = function (x, y) {
				count = 0;
				for (var i = 0; i < 47; i++) {
					count -= Math.abs(x.charCodeAt(i)-y.charCodeAt(i));
					if (x.substring(i, i+1) == y.substring(i, i+1)) {
						count += 100;
					}
				}
				return count;
			}
			
			var cycle = 20;
			var first = true;
			var found = false;
			var abc = 1;
			var top1, top2, top1score, top2score, i, strings, stop;
			var total = 0;
			var finaloutput = '';
			function go() {
				var i, j, outputstrings = [];
				if (first) {
					first = false;
					strings = [];
					for (i = 0; i < cycle; i++) {
						var str = '';
						for (j = 0; j < 47; j++)
							str += randomLetter();
							
						strings[i] = str;
						outputstrings[i] = '<span style="color: #009E28">'+str+'</span>';
					}
				} else {
					// Generate new strings
					for (i = 0; i < cycle; i++) {
						if (i == 10) {
							// Swap strings
							var tmp = top1;
							top1 = top2;
							top2 = tmp;
						}
						splitAt = a.floor(a.random()*47);
						strings[i] = top1.substring(0, splitAt)+top2.substring(splitAt, 47);
		
						if (Math.random() < 0.4) {
							mutateAt = a.floor(a.random()*47)+1;
							strings[i] = strings[i].substring(0, mutateAt-1)+randomLetter()+strings[i].substring(mutateAt, 47)
							var os = '';
							if (mutateAt <= splitAt) {
								// split first
								if (i < 10)
									os = top1.substring(0, splitAt)+'</span><span style="color: #36FF33">'+top2.substring(splitAt, 47);
								else
									os = top1.substring(0, splitAt)+'</span><span style="color: #FAF4F4">'+top2.substring(splitAt, 47);

								os = os.substring(0, mutateAt-1)+'<span style="background-color: #FFFFB3">'+strings[i].charAt(mutateAt-1)+'</span>'+os.substring(mutateAt, os.length);
//os = strings[i]+"-"+os;

							} else {
								// mutate first
								os = strings[i].substring(0, mutateAt-1)+'<span style="background-color: #FFFFB3">'+strings[i].charAt(mutateAt-1)+'</span>'+strings[i].substring(mutateAt, 47)
								if (i < 10)
									os = os.substring(0, splitAt)+'</span><span style="color: #36FF33">'+os.substring(splitAt, os.length);
								else	
									os = os.substring(0, splitAt)+'</span><span style="color: #FAF4F4">'+os.substring(splitAt, os.length);
//os = strings[i]+"+"+os;
							}
							if (i < 10)
								outputstrings[i] = '<span style="color: #FAF4F4">'+os+'</span>';
							else
								outputstrings[i] = '<span style="color: #36FF33">'+os+'</span>';

						} else {
							if (i < 10)
								outputstrings[i] = '<span style="color: #FAF4F4">'+top1.substring(0, splitAt)+'</span><span style="color: #36FF33">'+top2.substring(splitAt, 47)+'</span>';
							else
								outputstrings[i] = '<span style="color: #36FF33">'+top1.substring(0, splitAt)+'</span><span style="color: #FAF4F4">'+top2.substring(splitAt, 47)+'</span>';
						}
					}
				}
				total += cycle;
				top1 = strings[0];
				top2 = strings[1];
				top1score = top2score = -10000;

				// Find top 2
				for (i = 0; i < cycle; i++) {
					score = similarString(strings[i], b);
					if (score > top1score) {
						top1 = strings[i];
						top1score = score;
						if (top1 == "Ingeniería informática, inteligencia artificial") {
							document.getElementById('output').innerHTML = b;
							total = (total - cycle) + i;
							document.getElementById('stats').innerHTML = " [ "+total+" Cantidad de cadenas geneticamente modificadas. ]";
							finaloutput = document.getElementById('all').innerHTML+finaloutput;
							document.getElementById('all').innerHTML = '<pre style="width: 45em; margin: 10px auto; text-align: center"><span style="color: #FCFEFC">'+top1+'</span> <span style="color: #36FF33">'+top2+'</span></pre>'+
                                '<pre style="-moz-column-width: 2em;-webkit-column-width: 12em;-moz-column-gap: 2em;-webkit-column-gap: 12em; width: 87em; text-align: center; margin: 10px">'+outputstrings.join('\n')+'</pre>';
                            setTimeout(function () { document.getElementById('all').innerHTML = document.getElementById('all').innerHTML+finaloutput }, 50);

							clearInterval(stop);
							return;
						}
					} else if (score > top2score) {
						top2 = strings[i];
						top2score = score;
					}
				}
				
				document.getElementById('output').innerHTML = top1;
				finaloutput = document.getElementById('all').innerHTML+finaloutput;
				document.getElementById('all').innerHTML = '<pre style="width: 45em; margin: 20px auto; text-align: center"><span style="color: #FCFEFC">'+top1+'</span> <span style="color: #36FF33 ">'+top2+'</span></pre>'+
                    '<pre style="column-count: 2; column-gap: 10em;column-width: 10em;-moz-column-width: 10em;-webkit-column-width: 10em;-moz-column-gap: 10em;-webkit-column-gap: 10em; width: 45em; text-align: center; margin: 10px auto;">'+outputstrings.join('\n')+'</pre>'//+document.getElementById('all').innerHTML;
				document.getElementById('stats').innerHTML = " [ "+total+" Cantidad de cadenas geneticamente modificadas. ]";
			}
			stop = setInterval(function () { go() }, 50);
		</script>
		<pre style="margin: 40px; font-size: 40px; font-family: open sans; text-align: center; border: 5px solid #595260;
		background:#b1b2b9;height:60px;width: 1290px;padding: 10px;margin: 10px;" id="output"></pre>
		<div style="font-size: 2em; font-family: open sans; text-align: center; background-color: aliceblue;" id="stats"></div>
		<div id="all"></div>
	</body>
</html>
