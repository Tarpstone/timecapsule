<script>
	import NavOne from '$lib/deep/NavOne.svelte';
	import SeoDeep from '$lib/deep/SeoDeep.svelte';

	//Trembling message script- ' Dynamic Drive (www.dynamicdrive.com)
	//For full source code, 100's more DHTML scripts, and TOS,
	//visit https://www.dynamicdrive.com
	//<script language="JavaScript1.2">
	function scriptOne() {
		var ns6 = document.getElementById && !document.all;
		var ie = document.all;

		var customcollect = new Array();
		var i = 0;

		function jiggleit(num) {
			if (!document.all && !document.getElementById) return;
			customcollect[num].style.left =
				parseInt(customcollect[num].style.left) == -1
					? (customcollect[num].style.left = 1)
					: (customcollect[num].style.left = -1);
		}

		function init() {
			if (ie) {
				while (eval('document.all.jiggle' + i) != null) {
					customcollect[i] = eval('document.all.jiggle' + i);
					i++;
				}
			} else if (ns6) {
				while (document.getElementById('jiggle' + i) != null) {
					customcollect[i] = document.getElementById('jiggle' + i);
					i++;
				}
			}

			if (customcollect.length == 1) setInterval('jiggleit(0)', 80);
			else if (customcollect.length > 1)
				for (y = 0; y < customcollect.length; y++) {
					var tempvariable = 'setInterval("jiggleit(' + y + ')",' + '100)';
					eval(tempvariable);
				}
		}

		window.onload = init;
	}

	/*
    Elastic Trail script (By Philip Winston @ pwinston@yahoo.com, URL: https://www.geocities.com/pwinston/)
    Script featured on Dynamicdrive.com
    For this and 100's more DHTML scripts, visit https://dynamicdrive.com
    */
	//<script LANGUAGE="JavaScript">
	function scriptTwo() {
		var nDots = 7;

		var Xpos = 0;
		var Ypos = 0;

		// fixed time step, no relation to real time
		var DELTAT = 0.01;
		// size of one spring in pixels
		var SEGLEN = 10;
		// spring constant, stiffness of springs
		var SPRINGK = 10;
		// all the physics is bogus, just picked stuff to
		// make it look okay
		var MASS = 1;
		// Positive XGRAVITY pulls right, negative pulls left
		// Positive YGRAVITY pulls down, negative up
		var XGRAVITY = 0;
		var YGRAVITY = 50;
		// RESISTANCE determines a slowing force proportional to velocity
		var RESISTANCE = 10;
		// stopping criterea to prevent endless jittering
		// doesn't work when sitting on bottom since floor
		// doesn't push back so acceleration always as big
		// as gravity
		var STOPVEL = 0.1;
		var STOPACC = 0.1;
		var DOTSIZE = 11;
		// BOUNCE is percent of velocity retained when
		// bouncing off a wall
		var BOUNCE = 0.75;

		var isNetscape = navigator.appName == 'Netscape';

		// always on for now, could be played with to
		// let dots fall to botton, get thrown, etc.
		var followmouse = true;

		var dots = new Array();
		init();

		function init() {
			var i = 0;
			for (i = 0; i < nDots; i++) {
				dots[i] = new dot(i);
			}

			if (!isNetscape) {
				// I only know how to read the locations of the
				// <LI> items in IE
				//skip this for now
				// setInitPositions(dots)
			}

			// set their positions
			for (i = 0; i < nDots; i++) {
				dots[i].obj.left = dots[i].X;
				dots[i].obj.top = dots[i].Y;
			}

			if (isNetscape) {
				// start right away since they are positioned
				// at 0, 0
				startanimate();
			} else {
				// let dots sit there for a few seconds
				// since they're hiding on the real bullets
				setTimeout('startanimate()', 1000);
			}
		}

		function dot(i) {
			this.X = Xpos;
			this.Y = Ypos;
			this.dx = 0;
			this.dy = 0;
			if (isNetscape) {
				this.obj = eval('document.dot' + i);
			} else {
				this.obj = eval('dot' + i + '.style');
			}
		}

		function startanimate() {
			setInterval('animate()', 20);
		}

		// This is to line up the bullets with actual LI tags on the page
		// Had to add -DOTSIZE to X and 2*DOTSIZE to Y for IE 5, not sure why
		// Still doesn't work great
		function setInitPositions(dots) {
			// initialize dot positions to be on top
			// of the bullets in the <ul>
			var startloc = document.all.tags('LI');
			var i = 0;
			for (i = 0; i < startloc.length && i < nDots - 1; i++) {
				dots[i + 1].X = startloc[i].offsetLeft;
				startloc[i].offsetParent.offsetLeft - DOTSIZE;
				dots[i + 1].Y = startloc[i].offsetTop + startloc[i].offsetParent.offsetTop + 2 * DOTSIZE;
			}
			// put 0th dot above 1st (it is hidden)
			dots[0].X = dots[1].X;
			dots[0].Y = dots[1].Y - SEGLEN;
		}

		// just save mouse position for animate() to use
		function MoveHandler(e) {
			Xpos = e.pageX;
			Ypos = e.pageY;
			return true;
		}

		// just save mouse position for animate() to use
		function MoveHandlerIE() {
			Xpos = window.event.x + document.body.scrollLeft;
			Ypos = window.event.y + document.body.scrollTop;
		}

		if (isNetscape) {
			document.captureEvents(Event.MOUSEMOVE);
			document.onMouseMove = MoveHandler;
		} else {
			document.onmousemove = MoveHandlerIE;
		}

		function vec(X, Y) {
			this.X = X;
			this.Y = Y;
		}

		// adds force in X and Y to spring for dot[i] on dot[j]
		function springForce(i, j, spring) {
			var dx = dots[i].X - dots[j].X;
			var dy = dots[i].Y - dots[j].Y;
			var len = Math.sqrt(dx * dx + dy * dy);
			if (len > SEGLEN) {
				var springF = SPRINGK * (len - SEGLEN);
				spring.X += (dx / len) * springF;
				spring.Y += (dy / len) * springF;
			}
		}

		function animate() {
			// dots[0] follows the mouse,
			// though no dot is drawn there
			var start = 0;
			if (followmouse) {
				dots[0].X = Xpos;
				dots[0].Y = Ypos;
				start = 1;
			}

			for (i = start; i < nDots; i++) {
				var spring = new vec(0, 0);
				if (i > 0) {
					springForce(i - 1, i, spring);
				}
				if (i < nDots - 1) {
					springForce(i + 1, i, spring);
				}

				// air resisitance/friction
				var resist = new vec(-dots[i].dx * RESISTANCE, -dots[i].dy * RESISTANCE);

				// compute new accel, including gravity
				var accel = new vec(
					(spring.X + resist.X) / MASS + XGRAVITY,
					(spring.Y + resist.Y) / MASS + YGRAVITY
				);

				// compute new velocity
				dots[i].dx += DELTAT * accel.X;
				dots[i].dy += DELTAT * accel.Y;

				// stop dead so it doesn't jitter when nearly still
				if (
					Math.abs(dots[i].dx) < STOPVEL &&
					Math.abs(dots[i].dy) < STOPVEL &&
					Math.abs(accel.X) < STOPACC &&
					Math.abs(accel.Y) < STOPACC
				) {
					dots[i].dx = 0;
					dots[i].dy = 0;
				}

				// move to new position
				dots[i].X += dots[i].dx;
				dots[i].Y += dots[i].dy;

				// get size of window
				var height, width;
				if (isNetscape) {
					height = window.innerHeight + window.pageYOffset;
					width = window.innerWidth + window.pageXOffset;
				} else {
					height = document.body.clientHeight + document.body.scrollTop;
					width = document.body.clientWidth + document.body.scrollLeft;
				}

				// bounce off 3 walls (leave ceiling open)
				if (dots[i].Y >= height - DOTSIZE - 1) {
					if (dots[i].dy > 0) {
						dots[i].dy = BOUNCE * -dots[i].dy;
					}
					dots[i].Y = height - DOTSIZE - 1;
				}
				if (dots[i].X >= width - DOTSIZE) {
					if (dots[i].dx > 0) {
						dots[i].dx = BOUNCE * -dots[i].dx;
					}
					dots[i].X = width - DOTSIZE - 1;
				}
				if (dots[i].X < 0) {
					if (dots[i].dx < 0) {
						dots[i].dx = BOUNCE * -dots[i].dx;
					}
					dots[i].X = 0;
				}

				// move img to new position
				dots[i].obj.left = dots[i].X;
				dots[i].obj.top = dots[i].Y;
			}
		}
		// end code hiding
	}

	// scriptOne();
	// scriptTwo();
</script>

<SeoDeep title="Mr. Man Careers - The Page of Insanity" description="" />
<div class="page-style" id="insanity-page-style">
	<NavOne />
	<main>
		<div id="dot0" style="position: absolute; visibility: hidden; height: 11; width: 11;">
			<img src="/timecapsuledeep/images/bullet.gif" height="11" width="11" />
		</div>
		<div id="dot1" style="position: absolute; height: 11; width: 11;">
			<img src="/timecapsuledeep/images/bullet.gif" height="11" width="11" />
		</div>
		<div id="dot2" style="position: absolute; height: 11; width: 11;">
			<img src="/timecapsuledeep/images/bullet.gif" height="11" width="11" />
		</div>
		<div id="dot3" style="position: absolute; height: 11; width: 11;">
			<img src="/timecapsuledeep/images/bullet.gif" height="11" width="11" />
		</div>
		<div id="dot4" style="position: absolute; height: 11; width: 11;">
			<img src="/timecapsuledeep/images/bullet.gif" height="11" width="11" />
		</div>
		<div id="dot5" style="position: absolute; height: 11; width: 11;">
			<img src="/timecapsuledeep/images/bullet.gif" height="11" width="11" />
		</div>
		<div id="dot6" style="position: absolute; height: 11; width: 11;">
			<img src="/timecapsuledeep/images/bullet.gif" height="11" width="11" />
		</div>

		<!-- <span id="jiggle0" class="jc"> -->
		<h1 class="purple">
			<center>tHE pAGe OF insAnITy</center>
		</h1>
		<img src="/timecapsuledeep/images/DoomWheel.gif" align="right" />
		<p class="white">
			WoW, lOOK at All oF thE rANdoM sTuff...hoW wiLL You eVer sUrviVe? (I'll just tell you: click
			the Home button. Duh...)
		</p>
		<center>
			<img src="/timecapsuledeep/images/SpinStar.gif" />
			<img src="/timecapsuledeep/images/CareerSwivel.gif" />
			<img src="/timecapsuledeep/images/HiFlag.gif" />
			<img src="/timecapsuledeep/images/RainbowStar.gif" />
		</center>
	</main>
	<footer>
		<p>
			Credit to <a href="https://www.dynamicdrive.com">Dynamic Drive</a>
			for the cool mouse ball chain on The Page of Insanity. Thanks a lot, DD!
		</p>

		<p>
			Also, thanks to <a href="https://www.astronomy.swin.edu.au/~pbourke/fractals/">
				Fractals, Chaos</a
			> for the background of The Page of Insanity.
		</p>

		<p>But credit to me for the animations you see around the site. They're all mine. :)</p>
		<p>
			And one final note: if you don't recognize the name of some of the colleges and institutions
			that I talk about, you probably don't live in Virginia Beach. :)
		</p>
	</footer>
</div>

<style>
	#insanity-page-style {
		font-family: verdana;
	}

	.purple {
		color: purple;
	}

	.white {
		color: white;
	}

	footer {
		font-size: 2px;
	}

	main {
		display: block;
	}

	.jc {
		position: relative;
	}

	:global(body) {
		background-color: white;
		background-image: url('/timecapsuledeep/images/fractalbg.jpg');
	}
</style>
