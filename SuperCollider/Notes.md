Start audio server with:
```
s.boot;
```

## UGen
+ ar
+ kr
+ ir

Take inputs 

## SynthDef and Synth
```
(
SynthDef.new(\sineTest, {
	arg noiseHz=8;
	var freq, amp, sig;
	freq = LFNoise0.kr(noiseHz).exprange(200, 1000);
	amp = LFNoise1.kr(12).exprange(0.02, 1);
	sig = SinOsc.ar(freq) * amp;
	Out.ar(0, sig);
}).add;
)
```

First input `sineTest` is the name of the object. Can either be a string `"sineTest"` or a symbol `\sineTest`.

The second input is the `ugenGraphFunction`. This is simply a UGen function. `Out.ar(output, signal)` is needed for mapping to hardware outputs.

We then add the SynthDef.

To execute the SynthDef we create a new `Synth` and assign it to a global variable. We specify the SynthDef we are using in the first argument, and pass in an array of arguments to the SynthDef in our second argument.

```
x = Synth.new(\sineTest, [\noiseHz, 8]);
```

`x.set(args)` can be called to change arguments.

Here is the code I wrote for [SuperCollider Tutorial: 3. Synth and SynthDef](https://www.youtube.com/watch?v=LKGGWsXyiyo)

```
(
SynthDef.new(\pulseTest, {
	arg ampHz = 4, fund=40, maxPartial=4, width = 0.5;
	var amp1, amp2, freq1, freq2, sig1, sig2;
	amp1 = LFPulse.kr(ampHz, 0, 0.12) * 0.75;
	amp2 = LFPulse.kr(ampHz, 0.5, 0.12) * 0.75;
	freq1 = LFNoise0.kr(4).exprange(fund, fund*maxPartial).round(fund);
	freq2 = LFNoise0.kr(4).exprange(fund, fund*maxPartial).round(fund);
	freq1 = freq1 * LFPulse.kr(8, add:1);
	freq2 = freq1 * LFPulse.kr(6, add:1);
	sig1 = Pulse.ar(freq1, width, amp1);
	sig2 = Pulse.ar(freq2, width, amp2);
	sig1 = FreeVerb.ar(sig1, 0.7, 0.8, 0.25);
	sig2 = FreeVerb.ar(sig2, 0.7, 0.8, 0.25);
	Out.ar(0, sig1);
	Out.ar(1, sig2);
}).add;
)

x = Synth.new(\pulseTest, [4, 40, 4, 0.5]);
x.set([5, 40, 4, 0.5]);
x.set(\width, 0.25);
x.set(\fund, 40);
x.set(\ampHz, 1);
x.free;
```

## Envelopes and doneAction

```
(
{
	var sig, freq, env;
	env = XLine.kr(1, 0.01, 5, doneAction:2);
	freq = XLine.kr(135, 110, 0.25, doneAction:0);
	sig = Pulse.ar(freq) * env;
}.play;
)

(
SynthDef.new(\envTest, {
	arg gate = 1;
	var sig, sig1, sig2, env, freq;
	e = Env.new([0, 1, 0.6, 0], [0.5, 1.5, 2], [3, 3, 0]);
	env = EnvGen.kr(e, doneAction:2);
	freq = EnvGen.kr(Env.adsr(1), gate, 200, 0.1);
	sig1 = Pulse.ar(freq) * env * 0.5;
	sig2 = Saw.ar(freq) * env * 0.2;
	sig1 = FreeVerb.ar(sig1, 0.7, 0.8, 0.25);
	sig2 = FreeVerb.ar(sig2, 0.8, 0.8, 0.25);
	sig = sig1 + sig2;
	Out.ar(0, sig);
	Out.ar(1, sig);
}).add;
)

x = Synth.new(\envTest)
```

