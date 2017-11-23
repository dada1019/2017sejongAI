     가설 1. Punctuation에 따른 감정변화를 파악할 수 있을 것이다.


written text는 말의 억양을 표현하지 못하기 때문에, 사람들은 punctuation이나 기호를 사용해서 문장에 감정을 담아내는 경우가 많다. 예를 들어, 느낌표를 여러 개 붙여서 사용하면 그 문장을 더욱 강조한 것이고, :)이나 :(와 같이 감정을 사용하는 특정 부호를 만들어서 덧붙일 수도 있다. nltk 감정 분석기가 이와 같은 punctuation에 따른 변화를 파악 할 수 있다는 가설을 세우고 리뷰를 테스트해보았다.

* 문장부호에 따른 감정변화를 체크해 보기 위해 똑같은 문장에 문장부호만 바꾸었다


결과

-- original
Review: 
"You should definitely watch if you are a true fan of Spider-Man and Marvel. There is no doubt that Tobey Maguire was a better actor."
   *Predicted sentiment: Positive
   *Probability: 0.69

-- test
Review: 
"You should definitely watch if you are a true fan of Spider-Man and Marvel!!!There is no doubt that Tobey Maguire was a better actor!!!"
   *Predicted sentiment: Positive
   *Probability: 0.69

Review: 
"You should definitely watch if you are a true fan of Spider-Man and Marvel~ There is no doubt that Tobey Maguire was a better actor~"
   *Predicted sentiment: Positive
   *Probability: 0.69

Review: 
"You should definitely watch if you are a true fan of Spider-Man and Marvel:)There is no doubt that Tobey Maguire was a better actor:)"
   *Predicted sentiment: Positive
   *Probability: 0.69

Review: 
"You should definitely watch if you are a true fan of Spider-Man and Marvel:< There is no doubt that Tobey Maguire was a better actor:("
   *Predicted sentiment: Positive
   *Probability: 0.69

-> 원래 리뷰와 테스트해본 리뷰가 모두 같은 probability를 보여준다. nltk 감정분석기는 punctuation에 따른 감정변화는 파악하지 못하며, 프로그램이 배운 단어들에 의해서만 감정분석이 가능한 것으로 밝혀졌다. 더불어, punctuation을 인식하지 않는 다는 말은 punctuation이 분석기를 돌림에 있어 아무런 영향을 주지 않는 다는 것을 뜻하기도 한다. 여러 가지 punctuation을 문장에 추가하더라도 같은 정확도와 probability를 보여준다.


     가설 2. 대소문자에 따른 강조여부를 파악할 수 있을 것이다.


앞의 가설과 비슷하게, 사람들은 수식어구를 대문자로 사용해서 그 단어를 강조하고, 더 깊은 감정을 표현하는 경우가 많다. 긍정적이거나 부정적인 수식어구를 대문자로 표기해서 강조했을 시, probability가 올라가는지 확인해보았다.


결과

-- original (negative)
Review:  
"Homecoming was TERRIBLE and by far the worst spider-man film. This movie was a HUGE set back for Spiderman but sadly we live in a very tight lip controlled marvel press."
   *Predicted sentiment: Negative
   *Probability: 0.85

-- test
Review:  
"Homecoming was terrible and by far the worst spider-man film. This movie was a huge set back for Spiderman but sadly we live in a very tight lip controlled marvel press"
   *Predicted sentiment: Negative
   *Probability: 0.96

-- original (positive)
Review: 
"You will enjoy EVERY single minute of this movie, Tom Holland is DEFINITELY the BEST peter Parker and Spider-Man we have ever seen."
   *Predicted sentiment: Negative
   *Probability: 0.51

-- test
Review: 
"You will enjoy every single minute of this movie, Tom Holland is definitely the best peter Parker and Spider-Man we have ever seen."
   *Predicted sentiment: Positive
   *Probability: 0.7

-> 감정을 나타내는 수식어구를 대문자로 사용해서 강조했을 때와 강조하지 않았을 때 서로 다른 probability를 보여주는 것으로 보아, nltk 감정분석기는 대문자를 인식해서 판단한다. 다만, 사람의 의도와는 다르게 대문자를 강조로 보지 않는 것 같다. 긍정일 경우와 부정일 경우 모두 대문자를 사용한 original text의 probability가 더 낮게 나왔기 때문이다. 글쓴이의 의도에 따르면 “Homecoming was TERRIBLE."이 "Homecoming was terrible."보다 terrible을 강조한 것으로, 앞문장의 probability가 더 높아야 하지만 반대의 경우가 도출되었기 때문이다. 심지어 3번째 문장인 positive의 경우, 0.51로 neutral하다는 판단을 내릴 정도로 오차가 크다.


     가설 3. 순위에 나오는 형용사가 많이 나올수록 probability가 높을 것이다.


순위가 높을수록 높은 긍정 혹은 부정을 표현해준다는 가설을 내렸다. 따라서 상위 15위 안에 드는 형용사들을 사용한 review를 찾아보고 감정분석기를 돌릴시, 1과 가까운 probability를 보여줄 것이다.

* 참고한 순위
Top 15 most informative words:
1. outstanding
2. insulting
3. vulnerable
4. ludicrous
5. uninvolving
6. avoids
7. astounding
8. fascination
9. darker
10. seagal
11. symbol
12. affecting
13. anna
14. animators
15. idiotic


결과

Review: 
"Homecominig is such an outstanding movie. Its story is fascinating. Every single acting was astounding."
   *Predicted sentiment: Positive
   *Probability: 0.94

Review: 
"The story was ludicrous and idiotic. Each scean does not connect to each other. It even seemed insulting."
   *Predicted sentiment: Negative
   *Probability: 0.84

Review: 
"It is a fascinating movie, yet the story line is ludicrous."
   *Predicted sentiment: Positive
   *Probability: 0.75

-> 긍정인 경우와 부정인 경우 모두 1의 probabilitiy를 보여주지는 않는다. 다만 순위권에 속해있는 단어가 probablity를 높여주는데 도움을 주는 것은 확실한데, (첫번째 review) 단어 3개가 속해있을 경우 0.94, (2번째 review) 단어 2개가 속해있을 경우 0.84로 단어가 더 많이 속해있을 때 probability가 더 높게 나타났기 때문이다. 
positive와 negative 단어가 각각 하나씩 들어갔을 경우, probability는 순위권의 영향을 받지 않는 것 같다. 3번째 review에서 fascinating이 ludicrous보다 아래에 머무르지만, nltk는 이를 positive하다고 판단했기 때문이다.
