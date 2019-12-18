[weberna.github.io](https://weberna.github.io/blog/2017/11/15/LSTM-Vanishing-Gradients.html "A View from the Backwards Pass")

#  Why LSTMs Stop Your Gradients From Vanishing

A View from the Backwards Pass



##  LSTMs: The Gentle Giants

On their surface, LSTMs (and related architectures such as GRUs) seems like wonky, overly complex contraptions. Indeed, at first it seems almost sacrilegious to add these bulky accessories to our beautifully elegant [Elman-style][1] recurrent neural networks (RNNs)! However, unlike bloated software (such as Skype), this extra complexity is warranted in the case of LSTMs (also unlike Skype is the fact that LSTM/GRUs usually [work pretty well][2]). If you have read any paper that appeared around 2015-2016 that uses LSTMs you probably know that LSTMS solve the vanishing gradient problem that had plagued vanilla RNNs before hand.

If you don't already know, the vanishing gradient problem arises when, during backprop, the error signal used to train the network exponentially decreases the further you travel backwards in your network. The effect of this is that the layers closer to your input don't get trained. In the case of RNNs (which can be unrolled and thought of as feed forward networks with shared weights) this means that you don't keep track of any long term dependencies. This is kind of a bummer, since the whole point of an RNN is to keep track of long term dependencies. The situation is analogous to having a video chat application that can't handle video chats!

Looking at these big pieces of machinery its hard to get a concrete understanding of exactly _why_ they solve the vanishing gradient problem. The purpose of this blog post is to <del>put it on my resume</del> give a brief explanation as to why LSTMs (and related models) solve the vanishing gradient problem. The reason _why_ is actually pretty simple, which is all the more reason to know it. If you are unfamiliar with LSTM models I would check out this [post][3].

**Notation** Notation is always the pain when describing LSTMs since there are so many variables. I will list all notation here for convienence:

* $E_t$ = Error at time $t$, assume $E_t$ is a function of output $y_t$
* $W_R$ = The recurrent set of weights (other sets of weights denoted with a different subscript)
* $tanh, \sigma$ = The activation function tanh, or sigmoid
* $h_t$ = The hidden vector at time $t$ 
* $ C_t$= The LSTM cell state at time $t$ 
* $o_t,f_t,i_t$ = The LSTM output, forget, and input gates at time $t$ 
* $x_t,y_t$ = The input and output at time $t$ 

**LSTM Equation Reference** Quickly, here is a little review of the LSTM equations, with the biases left off (and mostly the same notation as [Chris Olah's post][3]):


* $f_t=\sigma(W_f[h_{t-1},x_t])$
* $i_t=sigma(W_i[h_{t-1},x_t])$
* $o_t=sigma(W_o[h_{t-1},x_t])$
* $\widetilde{C}_t=tanh(W_C[h_{t-1},x_t])$
* $C_t=f_tC_{t-1} + i_t\widetilde{C}_t$
* $h_t=o_ttanh(C_t)$

##  The Case of the Vanishing Gradients

To understand why LSTMs help, we need to understand the problem with vanilla RNNs. In a vanilla RNN, the hidden vector and the output is computed as such:

$$h_t = tanh(W_Ix_t + W_Rh_{t-1})\\
y_t = W_Oh_t$$



To do backpropagation through time to train the RNN, we need to compute the gradient of $E$ with respect to $W_R$. The overall error gradient is equal to the sum of the error gradients at each time step. For step $t$, we can use the multivariate chain rule to derive the error gradient as:

$$\frac{\partial E_t}{\partial W_R} = \sum^{t}_{i=0} \frac{\partial E_t}{\partial y_t}\frac{\partial y_t}{\partial h_t}\frac{\partial h_t}{\partial h_i}\frac{\partial h_i}{\partial W_R}$$



Now everything here can be computed pretty easily _except_ the term $\frac{\partial h_t}{\partial h_i}$, which needs another chain rule application to compute:

$$\frac{\partial h_t}{\partial h_i} = \frac{\partial h_t}{\partial h_{t-1}}\frac{\partial h_{t-1}}{\partial h_{t-2}}...\frac{\partial h_{i+1}}{\partial h_i} = \prod^{t-1}_{k=i} \frac{\partial h_{k+1}}{\partial h_k}$$



Now let us look at a single one of these terms by taking the derivative of $h_{k+1}$ with respect to $h_{k}$(where _diag_ turns a vector into a diagonal matrix)[1][4]:

$$\frac{\partial h_{k+1}}{\partial h_k} =  diag(f'(W_Ix_i + W_Rh_{i-1}))W_R$$



Thus, if we want to backpropagate through $k$ timesteps, this gradient will be :

$$\frac{\partial h_{k}}{\partial h_1} =  \prod_i^k diag(f'(W_Ix_i + W_Rh_{i-1}))W_R$$

As shown in [this paper][5], if the dominant eigenvalue of the matrix $W_R$ is greater than 1, the gradient explodes. If it is less than 1, the gradient vanishes.[2][6] The fact that this equation leads to either vanishing or exploding gradients should make intuitive sense. Note that the values of $f'(x)$ will always be less than 1. So if the magnitude of the values of $W_R$ are too small, then inevitably the derivative will go to 0. The repeated multiplications of values less than one would overpower the repeated multiplications of $W_R$. On the contrary, make $W_R$ _too_ big and the derivative will go to infinity since the exponentiation of $W_R$ will overpower the repeated multiplication of the values less than 1. In practice, the vanishing gradient is more common, so we will mostly focus on that.

The derivative $\frac{\partial h_{k}}{\partial h_1}$ is essentially telling us how much our hidden state at time $k$ will change when we change the hidden state at time 1 by a little bit. According to the above math, if the gradient vanishes it means the earlier hidden states have no real effect on the later hidden states, meaning no long term dependencies are learned! This can be formally proved, and has been in [many papers][5], including the [original LSTM paper][7].



##  Preventing Vanishing Gradients with LSTMs

As we can see above, the biggest culprit in causing our gradients to vanish is that dastardly recursive derivative we need to compute: $\frac{\partial h_t}{\partial h_i}$. If only this derivative was 'well behaved' (that is, it doesn't go to 0 or infinity as we backpropagate through layers) then we could learn long term dependencies!



**The original LSTM solution** The original motivation behind the LSTM was to make this recursive derivative have a constant value. If this is the case then our gradients would neither explode or vanish. How is this accomplished? As you may know, the LSTM introduces a separate cell state $C_t$. In the original 1997 LSTM, the value for $C_t$ depends on the previous value of the cell state and an update term weighted by the input gate value (for motivation on why the input/output gates are needed, I would check out [this great post][8]):
$$C_t = C_{t-1} + i\widetilde{C}_t$$

This formulation doesn't work well because the cell state tends to grow uncontrollably. In order to prevent this unbounded growth, a [forget gate was added][9] to scale the previous cell state, leading to the more modern formulation:
$$C_t = fC_{t-1} + i\widetilde{C}_t$$

**A common misconception** Most explanations for why LSTMs solve the vanishing gradient state that under this update rule, the recursive derivative is equal to 1 (in the case of the original LSTM) or $f$ (in the case of the modern LSTM)[3][10] and is thus well behaved! One thing that is often forgotten is that $f$, $i$, and $\widetilde{C}_t$ are all functions of $C_t$, and thus we must take them into consideration when calculating the gradient.

The reason for this misconception is pretty reasonable. In the original LSTM formulation in 1997, the recursive gradient actually was equal to 1. The reason for this is because, in order to enforce this constant error flow, the gradient calculation was truncated so as not to flow back to the input or candidate gates. So with respect to $C_{t-1}$ they could be treated as constants. Here what they say in the [original paper][7]:

> However, to ensure non-decaying error backprop through internal states of memory cells, as with truncated BPTT (e.g.,Williams and Peng 1990), errors arriving at "memory cell net inputs" [the cell output, input, forget, and candidate gates] …do not get propagated back further in time (although they do serve to change the incoming weights).Only within memory cells [the cell state],errors are propagated back through previous internal states.

In fact truncating the gradients in this way was done up till about 2005, until the publication of [this paper][11] by Alex Graves. Since most popular neural network frameworks now do auto differentiation, its likely that you are using the full LSTM gradient formulation too! So, does the above argument about why LSTMs solve the vanishing gradient change when using the full gradient? The answer is no, actually it remains mostly the same. It just gets a bit messier.

**Looking at the full LSTM gradient**[4][12] To understand why nothing really changes when using the full gradient, we need to look at what happens to the recursive gradient when we take the full gradient. As we stated before, the recursive derivative is the main thing that is causing the vanishing gradient, so lets expand out the full derivative for $\frac{\partial C_t}{\partial C_{t-1}}$. First recall that in the LSTM, $C_t$ is a function of $f_t$ (the forget gate), $i_t$ (the input gate), and $\widetilde{C}_t$ (the candidate cell state), each of these being a function of $C_{t-1}$ (since they are all functions of $h_{t-1}$). Via the multivariate chain rule we get:
$$% <![CDATA[
\begin{align*}
\frac{\partial C_t}{\partial C_{t-1}} &= \frac{\partial C_t}{\partial f_{t}}\frac{\partial f_t}{\partial h_{t-1}}\frac{\partial h_{t-1}}{\partial C_{t-1}}  + \frac{\partial C_t}{\partial i_{t}}\frac{\partial i_t}{\partial h_{t-1}}\frac{\partial h_{t-1}}{\partial C_{t-1}} \\
 &+ \frac{\partial C_t}{\partial \widetilde{C}_{t}}\frac{\partial \widetilde{C}_t}{\partial h_{t-1}}\frac{\partial h_{t-1}}{\partial C_{t-1}} + \frac{\partial C_t}{\partial C_{t-1}}
\end{align*} %]]>$$



Now lets explicitly write out these derivatives:
$$% <![CDATA[
\begin{align*}
\frac{\partial C_t}{\partial C_{t-1}} &= C_{t-1}\sigma'(\cdot)W_f*o_{t-1}tanh'(C_{t-1}) \\
&+ \widetilde{C}_t\sigma'(\cdot)W_i*o_{t-1}tanh'(C_{t-1}) \\
&+ i_t\tanh'(\cdot)W_C*o_{t-1}tanh'(C_{t-1}) \\
&+ f_t
\end{align*} %]]>$$



Now if we want to backpropagate back $k$ time steps, we simply multiply terms in the form of the one above $k$ times. Note the big difference between this recursive gradient and the one for vanilla RNNs. In vanilla RNNs, the terms $\frac{\partial h_t}{\partial h_{t-1}}$ will eventually take on a values that are either always above 1 or always in the range $[0,1]$, this is essentially what leads to the vanishing/exploding gradient problem. The terms here, $\frac{\partial C_t}{\partial C_{t-1}}$, _at any time step_ can take on either values that are greater than 1 or values in the range $[0,1]$. Thus if we extend to an infinite amount of time steps, it is not guarenteed that we will end up converging to 0 or infinity (unlike in vanilla RNNs). If we start to converge to zero, we can always set the values of $f_t$ (and other gate values) to be higher in order to bring the value of $\frac{\partial C_t}{\partial C_{t-1}}$ closer to 1, thus preventing the gradients from vanishing (or at the very least, preventing them from vanishing _too_ quickly). One important thing to note is that the values $f_t$, $o_t$, $i_t$, and $\widetilde{C}_t$ are things that the network _learns_ to set (conditioned on the current input and hidden state). Thus, in this way the network learns to decide _when_ to let the gradient vanish, and _when_ to preserve it, by setting the gate values accordingly!

This might all seem magical, but it really is just the result of two main things:

* The additive update function for the cell state gives a derivative thats much more 'well behaved'
* The gating functions allow the network to decide how much the gradient vanishes, and can take on different values at each time step. The values that they take on are learned functions of the current input and hidden state.

And that is essentially it. It is good to know that truncating the gradient (as done in the original LSTM) is not too integral to explaining why the LSTM can prevent the vanishing gradient. As we see, the arguments for why it prevents the vanishing gradient remain somewhat similar even when taking the full gradient into account. Thanks for <del>reading</del> <del>skimming</del> scrolling to the bottom to look at the comments.




[1]: https://tatar.ucsd.edu/jeffelman/
[2]: http://karpathy.github.io/2015/05/21/rnn-effectiveness/
[3]: http://colah.github.io/posts/2015-08-Understanding-LSTMs/
[4]: https://weberna.github.io#fn%3A1
[5]: https://arxiv.org/pdf/1211.5063.pdf
[6]: https://weberna.github.io#fn%3A4
[7]: http://www.bioinf.jku.at/publications/older/2604.pdf
[8]: https://r2rt.com/written-memories-understanding-deriving-and-extending-the-lstm.html
[9]: https://pdfs.semanticscholar.org/1154/0131eae85b2e11d53df7f1360eeb6476e7f4.pdf
[10]: https://weberna.github.io#fn%3A2
[11]: ftp://ftp.idsia.ch/pub/juergen/nn_2005.pdf
[12]: https://weberna.github.io#fn%3A3
[13]: https://disqus.com/?ref_noscript

