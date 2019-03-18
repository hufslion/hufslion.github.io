---
layout: post
title: 인공지능으로 작곡하기.
category: test
---
예비 세션 2 - prod. 승윤

# 인공지능(마르코프 체인(Markov Chain))으로 작곡하기. 
---

## 마르코프 체인(Markov Chain)이란?
<a href="https://ko.wikipedia.org/wiki/%EB%A7%88%EB%A5%B4%EC%BD%94%ED%94%84_%EC%97%B0%EC%87%84">마르코프 체인 </a>

* 확률 과정의 일종으로, 각 시행의 결과가 바로 앞의 시행의 결과에만 영향을 받는 일련의 확률적 시행을 말한다.
* 어렵고 복잡하지만, 작곡 프로그램에 적용하는 방법은 간단하다. 칠판으로 적용 과정 설명.

## 활용할 패키지(Package).

* pysynth ( 음악 노트를 만들면, wav 파일로 저장해주는 패키지)
<https://github.com/mdoege/PySynth>

* pyknon ( 음악 노트를 쉽게 만들고, midi 파일로 저장해주는 기능을 가지고 있다)
<https://github.com/kroger/pyknon>

* MarkovChain 알고리즘 코드 ( 마르코프 체인 알고리즘이 구현된 파이썬 코드 활용)
<https://github.com/jcbozonier/MarkovMusic> 현재 파이썬 버전에 맞게 변형.

*필요한 패키지 모두 설치하기 (MarkovChain 코드는 따로 긁어오기로 한다.)
{% highlight html %}
pip install pysynth
pip install pyknon
{% endhighlight %}

* 추가적으로 만든 Mid 파일을 재생할 사이트.
<https://onlinesequencer.net/>


## 패키지 기본 활용.

* 간단한 음악 노트를 만들어복소 wav 파일로 저장하여 재생해 보자.
* 빈폴더를 만들고, VS코드로 열고 폴더하나 더 생성하기. 
* 만든 폴더에 main.py, MarkovBuilder.py, MarkovMusic.py 파일 생성하기.

*MarkovBuilder.py 로직 코드 삽입.

{% highlight html %}

import random

class MarkovBuilder:
    def __init__(self, value_list):
        self._values_added = 0
        self._reverse_value_lookup = value_list
        self._value_lookup = {}
        for i in range(0, len(value_list)):
            self._value_lookup[value_list[i]] = i
        #Initialize our adjacency matrix with the initial
        #probabilities for note transitions.
        self._matrix=[[0 for x in range(0,len(value_list))] for i in range(0,len(value_list))]

    def add(self, from_value, to_value):
        """Add a path from a note to another note. Re-adding a path between notes will increase the associated weight."""
        value = self._value_lookup
        self._matrix[value[from_value]][value[to_value]] += 1
        self._values_added = self._values_added + 1
        
    def next_value(self, from_value):
        value = self._value_lookup[from_value]
        value_counts = self._matrix[value]
        value_index = self.randomly_choose(value_counts)
        if(value_index < 0):
            raise RuntimeError("Non-existent value selected.")
        else:
            return self._reverse_value_lookup[value_index]
            
    def randomly_choose(self, choice_counts):
        """Given an array of counts, returns the index that was randomly chosen"""
        counted_sum = 0
        count_sum = sum(choice_counts)

        if count_sum == 0:
            return random.randint(0, len(choice_counts)-1)
        else:
            selected_count = random.randrange(1, count_sum + 1)
            for index in range(0, len(choice_counts)):
                counted_sum += choice_counts[index]
                if(counted_sum >= selected_count):
                    return index
        raise RuntimeError("Impossible value selection made. BAD!")
{% endhighlight %}


*MarkovMusic.py 로직 코드 삽입.

{% highlight html %}

import pysynth
import numpy as np
from MarkovBuilder import MarkovBuilder

class MusicMatrix:
    def __init__(self, song=None):
        self._previous_note = None

        if song is not None:
            notes = np.array(song, dtype=str)[:, 0]
            durations = np.array(song, dtype=str)[:, 1]

            for i, d in enumerate(durations):
                durations[i] = self.float2str(durations[i])

            self._markov = MarkovBuilder(np.unique(notes).tolist())
            self._timings = MarkovBuilder(np.unique(durations).tolist())

            for note in song:
                self.add(note)
        else:
            self._markov = MarkovBuilder(["a", "a#", "b", "c", "c#", "d", "d#", "e", "f", "f#", "g", "g#"])
            self._timings = MarkovBuilder([1, 2, 4, 8, 16])

        # print(self._markov._value_lookup)
        # print(self._timings._value_lookup)

    def float2str(self, d):
        if float(d) >= 1:
            return '%d' % int(float(d))
        else:
            return '%.2f' % float(d)

    def add(self, to_note):
        """Add a path from a note to another note. Re-adding a path between notes will increase the associated weight."""

        to_note = list(to_note)
        to_note[1] = self.float2str(to_note[1])

        if(self._previous_note is None):
            self._previous_note = to_note
            return
        from_note = self._previous_note
        self._markov.add(from_note[0], to_note[0])
        self._timings.add(from_note[1], to_note[1])
        self._previous_note = to_note
        
    def next_note(self, from_note):
        from_note = list(from_note)
        from_note[1] = self.float2str(from_note[1])

        return [self._markov.next_value(from_note[0]), float(self._timings.next_value(from_note[1]))]

if __name__ == "__main__":
    # Playing it comes next :)
    #test = [['c',4], ['e',4], ['g',4], ['c5',1]]
    #pysynth.make_wav(test, fn = "test.wav")

    musicLearner = MusicMatrix()

    # Input the melody of Row, Row, Row Your Boat
    # The MusicMatrix will automatically use this to 
    # model our own song after it.
    musicLearner.add(["c", 4])
    musicLearner.add(["c", 4])
    musicLearner.add(["c", 4])
    musicLearner.add(["d", 8])
    musicLearner.add(["e", 4])
    musicLearner.add(["e", 4])
    musicLearner.add(["d", 8])
    musicLearner.add(["e", 4])
    musicLearner.add(["f", 8])
    musicLearner.add(["g", 2])

    musicLearner.add(["c", 8])
    musicLearner.add(["c", 8])
    musicLearner.add(["c", 8])

    musicLearner.add(["g", 8])
    musicLearner.add(["g", 8])
    musicLearner.add(["g", 8])

    musicLearner.add(["e", 8])
    musicLearner.add(["e", 8])
    musicLearner.add(["e", 8])

    musicLearner.add(["c", 8])
    musicLearner.add(["c", 8])
    musicLearner.add(["c", 8])

    musicLearner.add(["g", 4])
    musicLearner.add(["f", 8])
    musicLearner.add(["e", 4])
    musicLearner.add(["d", 8])
    musicLearner.add(["c", 2])

    random_score = []
    current_note = ["c", 4]
    for i in range(0,100):
        print(current_note[0] + ", " + str(current_note[1]))
        current_note = musicLearner.next_note(current_note)
        random_score.append(current_note)

    pysynth.make_wav(random_score, fn = "first_score.wav")
{% endhighlight %}

* main.py로 기본 동요를 wav파일로 저장하고 실행해보자.

{% highlight html %}
import pysynth as ps
from pyknon.genmidi import Midi
from pyknon.music import NoteSeq, Note, Rest
from pprint import pprint
from MarkovMusic import MusicMatrix

song = [['c4', 4], ['c4', 4], ['c4', 4], ['d4', 8], ['e4', 4], ['e4', 4], ['d4', 8], ['e4', 4], ['f4', 8], ['g4', 2], ['c4', 8], ['c4', 8], ['c4', 8], ['g4', 8], ['g4', 8], ['g4', 8], ['e4', 8], ['e4', 8], ['e4', 8], ['c4', 8], ['c4', 8], ['c4', 8], ['g4', 4], ['f4', 8], ['e4', 4], ['d4', 8], ['c4', 2]]

ps.make_wav(song, fn='<저장할 파일 경로>/<파일 이름>.wav')

{% endhighlight %}

* 주의! 경로 설정할 때 경로 폴더를 긁어오고, 역슬래쉬를 슬래쉬로 바꿔주자.


## 마르코프 체인 알고리즘을 사용하여, 동요를 바탕으로 작곡을 해 보자. (마르코프 체인이 인공지능 작곡에 활용되는 원리 칠판 설명)

* MarkovMusic.py 파일 로직 설명

* MarkovBuilder.py 파일 로직 설명

* Midi 파일을 생성해주는 함수 추가. (없어도 되지만 편의를 위해서) 코드 부가 설명 

{% highlight html %}

def make_midi(midi_path, notes, bpm=120):
    note_names = 'c c# d d# e f f# g g# a a# b'.split()

    result = NoteSeq()
    for n in notes:
        duration = 1. / n[1]

        if n[0].lower() == 'r':
            result.append(Rest(dur=duration))
        else:
            pitch = n[0][:-1]
            octave = int(n[0][-1]) + 1
            pitch_number = note_names.index(pitch.lower())

            result.append(Note(pitch_number, octave=octave, dur=duration))

    midi = Midi(1, tempo=bpm)
    midi.seq_notes(result, track=0)
    midi.write(midi_path)        

{% endhighlight %}


* 코드 추가하기
{% highlight html %}
matrix = MusicMatrix(song) # 마르코프 체인 매트릭스에 만든 악보 적용하기.

start_note = ['c4', 4] # 처음 시작할 음 알려주기.

random_song = []
for i in range(0, 100):     # 마르코프 체인을 바탕으로 100번 다음 나올 음을 예측해서 악보를 만든다.
    start_note = matrix.next_note(start_note)
    random_song.append(start_note)

# ps.make_wav(random_song, fn='examples/random.wav')
make_midi(midi_path='midi/random_rowboat.mid', notes=random_song)

{% endhighlight %}

* 코드 설명

* row boat 동요를 바탕으로 인공지능이 작곡한 음악을 들어보자. (그럴듯 하게 나온다)


## 원리를 이해했으니 이제 다양한 기저 음악을 넣어 작곡을 시켜보자 - (Undertail).

* undertail 이라는 음악을 활용해 보자.
* undertail wav파일로 만들어서 들어보기.
* 아래코드로 작곡시키기.
* 만들어진 악보를 onlinesequencer.net 사이트에서 재생시켜보자.

{% highlight html %}
song = [('g#3', 16.0), ('e4', 16.0), ('d#4', 16.0), ('d4', 16.0), ('d#4', 16.0), ('r', 16.0), ('c#4', 16.0), ('b3', 16.0), ('a#3', 16.0), ('r', 16.0), ('g#3', 16.0), ('g3', 16.0), ('g#3', 16.0), ('r', 16.0), ('d#3', 16.0), ('e3', 16.0), ('d#3', 16.0), ('e3', 16.0), ('d#3', 16.0), ('d3', 16.0), ('d#3', 16.0), ('g3', 16.0), ('b3', 16.0), ('a#3', 16.0), ('g#3', 16.0), ('r', 16.0), ('g#3', 16.0), ('a#3', 16.0), ('b3', 16.0), ('r', 16.0), ('a#3', 16.0), ('b3', 16.0), ('c#4', 16.0), ('r', 16.0), ('b3', 16.0), ('a#3', 16.0), ('g3', 16.0), ('r', 16.0), ('g3', 16.0), ('a#3', 16.0), ('b3', 16.0), ('r', 16.0), ('a#3', 16.0), ('g#3', 16.0), ('d#3', 16.0), ('r', 16.0), ('d#3', 16.0), ('e3', 16.0), ('d#3', 16.0), ('e3', 16.0), ('d#3', 16.0), ('d3', 16.0), ('d#3', 16.0), ('g3', 16.0), ('b3', 16.0), ('a#3', 16.0), ('g#3', 16.0), ('r', 16.0), ('g#3', 16.0), ('g3', 16.0), ('g#3', 16.0), ('r', 16.0), ('g#4', 16.0)]

# ps.make_wav(song, fn='examples/undertail.wav')

matrix = MusicMatrix(song)

start_note = ['d4', 16]

random_song = []
for i in range(0, 100):
    start_note = matrix.next_note(start_note)
    random_song.append(start_note)

# ps.make_wav(random_song, fn='examples/random_undertail.wav')

make_midi(midi_path='midi/random_undertail.mid', notes=random_song)

{% endhighlight %}

## 다양한 음악 시도해보기 - 드뷔시(Debussy) - 꿈(Reverie)

{% highlight html %}
song = [('g5', 2.0), ('d5', 1.3333333333333333), ('e5', 8.0), ('f5', 8.0), ('g5', 4.0), ('e5', 8.0), ('d5', 8.0), ('e5', 5.333333333333333), ('c5', 5.314878892780648), ('e5', 5.314878892710022), ('d5', 1.0), ('a#4', 4.0), ('d5', 4.0), ('e5', 4.0), ('f5', 4.0), ('c5', 1.0), ('g4', 2.0), ('a4', 0.4999999999999998), ('a5', 2.0), ('e5', 1.3333333333333333), ('c5', 8.0), ('e5', 8.0), ('d5', 4.0), ('a#4', 8.0), ('g4', 8.0), ('a5', 2.0), ('e5', 1.3333333333333333), ('c5', 8.0), ('e5', 8.0), ('d5', 4.0), ('a#4', 8.0), ('g4', 8.0), ('g5', 2.0), ('d5', 1.3333333333333333), ('a#4', 8.0), ('d5', 8.0), ('c5', 4.0), ('a4', 8.0), ('g4', 8.0), ('a4', 4.0), ('e4', 8.0), ('a4', 8.0), ('f4', 4.0), ('d4', 8.0), ('f4', 8.0), ('d4', 2.0), ('c4', 2.000000000000007), ('c6', 2.0), ('g5', 1.3333333333333333), ('a5', 8.0), ('a#5', 8.0), ('c6', 4.0), ('a5', 8.0), ('g5', 8.0), ('a5', 5.333333333333333), ('f5', 5.314878892498194), ('a5', 5.314878892498194), ('g5', 1.0), ('d#5', 4.0), ('g5', 4.0), ('a5', 4.0), ('a#5', 4.0), ('f5', 1.0), ('a5', 4.0), ('a#5', 4.0), ('c6', 4.0), ('d6', 4.0), ('a5', 1.0), ('a#5', 4.0), ('d6', 4.0), ('f6', 2.0), ('c#6', 2.0000000003000054), ('r', 8.084210527894808), ('d5', 8.0), ('e5', 8.0), ('r', 4.0), ('f5', 8.0), ('a5', 8.0), ('f6', 2.0), ('c#6', 2.0), ('r', 4.0), ('d5', 8.0), ('e5', 8.0), ('r', 4.0), ('f5', 8.0), ('a5', 8.0), ('f5', 4.0), ('d4', 8.0), ('e4', 8.0), ('f5', 0.3333333333333333), ('f4', 8.0), ('a4', 8.0), ('f4', 2.0), ('f4', 1.0), ('d4', 4.0), ('c4', 4.0), ('d4', 1.0), ('c5', 4.0), ('a#4', 8.0), ('a4', 8.0), ('g4', 1.3333333333333333), ('a4', 8.0), ('a#4', 8.0), ('c5', 4.0), ('c5', 8.0), ('d#5', 8.0), ('d5', 0.5), ('c5', 4.0), ('a#4', 8.0), ('a4', 8.0), ('g4', 1.3333333333333333), ('a4', 8.0), ('a#4', 8.0), ('c5', 8.0), ('d#5', 8.0), ('e5', 0.6666666666666666), ('d#5', 4.0), ('e5', 4.0), ('g5', 4.0), ('f5', 8.0), ('e5', 8.0), ('d5', 2.0), ('d5', 2.0), ('e5', 4.0), ('c5', 0.08888888888888886), ('f5', 4.0), ('e5', 8.0), ('d5', 8.0), ('c5', 1.0), ('d5', 4.0), ('f5', 0.5), ('e5', 4.0), ('d5', 8.0), ('c5', 8.0), ('a#4', 2.0), ('a#5', 2.0), ('a5', 4.0), ('g5', 4.0), ('e5', 0.4444444444444444), ('f5', 0.6666666666666666), ('f4', 4.0), ('f5', 0.23529411764705882), ('e4', 4.0), ('f4', 16.0), ('a4', 16.0), ('f4', 16.0), ('e4', 4.0), ('d4', 4.0), ('c4', 2.0), ('e4', 4.0), ('c#4', 4.0), ('c#4', 4.0), ('e4', 4.0), ('f#4', 4.0), ('g#4', 4.0), ('e4', 2.0), ('f#5', 4.0), ('g#5', 4.0), ('e5', 4.0), ('c#6', 4.0), ('f#5', 4.0), ('g#5', 16.0), ('f#5', 16.0), ('g#5', 16.0), ('e5', 4.0), ('b4', 4.0), ('g#4', 4.0), ('b4', 4.0), ('g#4', 4.0), ('e4', 4.0), ('f#4', 2.0), ('c#5', 2.0), ('f#5', 4.0), ('g#5', 4.0), ('e5', 4.0), ('c#6', 4.0), ('f#5', 4.0), ('g#5', 16.0), ('f#5', 16.0), ('g#5', 16.0), ('e5', 4.0), ('b5', 4.0), ('c#6', 4.0), ('e6', 4.0), ('d#6', 4.0), ('b5', 4.0), ('c#6', 4.0), ('e6', 4.0), ('f#6', 4.0), ('b5', 4.000000000000114), ('c#6', 4.0), ('e6', 4.0), ('d#6', 4.0), ('b5', 4.0), ('c#6', 4.0), ('e6', 4.0), ('f#6', 4.0), ('b5', 4.0), ('g6', 1.3333333333333333), ('a5', 4.0), ('g5', 2.6666666666666665), ('a4', 8.0), ('g4', 2.6666666666666665), ('a4', 8.0), ('d4', 4.0), ('e4', 4.0), ('c4', 4.0), ('a4', 4.0), ('d4', 4.0), ('e4', 16.0), ('d4', 16.0), ('e4', 16.0), ('c4', 2.0), ('d5', 4.0), ('e5', 16.0), ('d5', 16.0), ('e5', 16.0), ('c5', 2.0), ('g5', 2.0), ('d5', 1.3333333333333333), ('e5', 8.0), ('f5', 8.0), ('g5', 4.0), ('e5', 8.0), ('d5', 8.0), ('e5', 5.333333333333333), ('c5', 5.31487889037946), ('e5', 5.31487889037946), ('d5', 1.0), ('a#4', 4.0), ('d5', 4.0), ('e5', 4.0), ('f5', 4.0), ('c5', 1.0), ('g4', 2.0), ('a4', 0.5), ('a5', 2.0), ('e5', 1.3333333333333333), ('c5', 8.0), ('e5', 8.0), ('d5', 4.0), ('a#4', 8.0), ('g4', 8.0), ('a5', 2.0), ('e5', 1.3333333333333333), ('c5', 8.0), ('e5', 8.0), ('d5', 4.0), ('a#4', 8.0), ('g4', 8.0), ('g5', 2.0), ('d5', 1.3333333333333333), ('a#4', 8.0), ('d5', 8.0), ('c5', 4.0), ('a4', 8.0), ('f4', 8.0), ('g5', 2.0), ('d5', 1.3333333333333333), ('a#4', 8.0), ('d5', 8.0), ('c5', 4.0), ('d5', 4.0), ('a#4', 4.0), ('e4', 0.02580385041830461), ('a4', 4.0), ('a4', 2.0), ('a#4', 4.0), ('a4', 4.0), ('a4', 2.0), ('a#4', 4.0), ('a#4', 0.6666666666666666), ('a4', 4.0), ('a#4', 16.0), ('d5', 16.0), ('a#4', 16.0), ('a4', 4.0), ('g4', 4.0), ('f4', 2.0), ('a#5', 4.0), ('a4', 0.49983729252717796), ('a5', 4.0), ('a5', 2.0), ('a#5', 4.0), ('a#5', 2.0), ('a5', 2.0), ('a#5', 4.0), ('a5', 4.0), ('a#5', 16.0), ('d6', 16.0), ('a#5', 16.0), ('a5', 2.0), ('g5', 2.0), ('a5', 0.36355029584697357), ('a6', 1.0)]

# ps.make_wav(song[:100], fn='examples/Debussy_Reverie.wav')

matrix = MusicMatrix(song)

start_note = ['g5', 2]

random_song = []
for i in range(0, 100):
    start_note = matrix.next_note(start_note)
    random_song.append(start_note)

# ps.make_wav(random_song, fn='examples/random_debussy.wav')
make_midi(midi_path='midi/random_debussy.mid', notes=random_song)
{% endhighlight %}

## 다양한 음악 시도해보기 - Synth Solo

{% highlight html %}

song = [('d3', 16.0), ('r', 16.0), ('g3', 16.0), ('r', 16.0), ('a3', 16.0), ('r', 16.0), ('d3', 16.0), ('r', 16.0), ('g3', 16.0), ('r', 16.0), ('a3', 16.0), ('r', 16.0), ('d3', 16.0), ('r', 16.0), ('d4', 16.0), ('r', 16.0), ('b3', 16.0), ('r', 16.0), ('d3', 16.0), ('r', 16.0), ('a3', 16.0), ('r', 16.0), ('g3', 16.0), ('r', 16.0), ('d3', 16.0), ('r', 16.0), ('a3', 16.0), ('r', 16.0), ('g3', 16.0), ('r', 16.0), ('d3', 16.0), ('r', 16.0), ('r', 5.333333333333333), ('f#3', 16.0), ('g3', 16.0), ('r', 16.0), ('d3', 16.0), ('r', 16.0), ('g3', 16.0), ('r', 16.0), ('a3', 16.0), ('r', 16.0), ('d3', 16.0), ('r', 8.0), ('f#3', 16.0), ('r', 16.0), ('f#3', 16.0), ('g3', 16.0), ('r', 16.0), ('a3', 16.0), ('r', 16.0), ('d3', 16.0), ('r', 16.0), ('b3', 16.0), ('r', 16.0), ('a3', 16.0), ('r', 16.0), ('g3', 16.0), ('r', 16.0), ('d3', 16.0), ('r', 16.0), ('f#3', 16.0), ('r', 16.0), ('g3', 16.0), ('r', 16.0), ('e3', 16.0), ('r', 16.0), ('g3', 16.0), ('r', 16.0), ('d4', 16.0), ('r', 16.0), ('f#3', 16.0), ('r', 16.0), ('g3', 16.0), ('r', 16.0), ('a3', 16.0), ('r', 16.0), ('f#3', 16.0), ('r', 16.0), ('g3', 16.0), ('r', 16.0), ('f#3', 16.0), ('r', 16.0), ('a3', 16.0), ('r', 16.0), ('g3', 16.0), ('r', 16.0), ('f#4', 16.0), ('r', 16.0), ('g4', 16.0), ('r', 16.0), ('g3', 16.0), ('r', 16.0), ('b3', 16.0), ('r', 16.0), ('d4', 16.0)]

# ps.make_wav(song[:100], fn='examples/synth_solo.wav')

matrix = MusicMatrix(song)

start_note = ['d4', 16]

random_song = []
for i in range(0, 100):
    start_note = matrix.next_note(start_note)
    random_song.append(start_note)

# ps.make_wav(random_song, fn='examples/random_synth_solo.wav')
make_midi(midi_path='midi/random_synth_solo.mid', notes=random_song)
{% endhighlight %}

## 두가지 음악 합쳐보기

{% highlight html %}
song1 = [['c4', 4], ['c4', 4], ['c4', 4], ['d4', 8], ['e4', 4], ['e4', 4], ['d4', 8], ['e4', 4], ['f4', 8], ['g4', 2], ['c4', 8], ['c4', 8], ['c4', 8], ['g4', 8], ['g4', 8], ['g4', 8], ['e4', 8], ['e4', 8], ['e4', 8], ['c4', 8], ['c4', 8], ['c4', 8], ['g4', 4], ['f4', 8], ['e4', 4], ['d4', 8], ['c4', 2]]

song2 = [('g#3', 16.0), ('e4', 16.0), ('d#4', 16.0), ('d4', 16.0), ('d#4', 16.0), ('r', 16.0), ('c#4', 16.0), ('b3', 16.0), ('a#3', 16.0), ('r', 16.0), ('g#3', 16.0), ('g3', 16.0), ('g#3', 16.0), ('r', 16.0), ('d#3', 16.0), ('e3', 16.0), ('d#3', 16.0), ('e3', 16.0), ('d#3', 16.0), ('d3', 16.0), ('d#3', 16.0), ('g3', 16.0), ('b3', 16.0), ('a#3', 16.0), ('g#3', 16.0), ('r', 16.0), ('g#3', 16.0), ('a#3', 16.0), ('b3', 16.0), ('r', 16.0), ('a#3', 16.0), ('b3', 16.0), ('c#4', 16.0), ('r', 16.0), ('b3', 16.0), ('a#3', 16.0), ('g3', 16.0), ('r', 16.0), ('g3', 16.0), ('a#3', 16.0), ('b3', 16.0), ('r', 16.0), ('a#3', 16.0), ('g#3', 16.0), ('d#3', 16.0), ('r', 16.0), ('d#3', 16.0), ('e3', 16.0), ('d#3', 16.0), ('e3', 16.0), ('d#3', 16.0), ('d3', 16.0), ('d#3', 16.0), ('g3', 16.0), ('b3', 16.0), ('a#3', 16.0), ('g#3', 16.0), ('r', 16.0), ('g#3', 16.0), ('g3', 16.0), ('g#3', 16.0), ('r', 16.0), ('g#4', 16.0)]

song = list(song1 * 2) + list(song2)

matrix = MusicMatrix(song)

pprint(matrix._markov._matrix)
pprint(matrix._timings._matrix)

start_note = ['e3', 8]

random_song = []
for i in range(0, 500):
    start_note = matrix.next_note(start_note)
    random_song.append(start_note)

# ps.make_wav(random_song, fn='examples/random_mix.wav')
make_midi(midi_path='midi/random_mix.mid', notes=random_song)
{% endhighlight %}


# PPT참고

[인공지능으로 작곡하기.pdf](https://github.com/seungyuns/newbitonproject/files/2975690/sub_session2_AI_composition.pdf)

