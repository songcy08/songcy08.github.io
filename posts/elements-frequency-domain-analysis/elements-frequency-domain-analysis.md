# Elements of Frequency-domain Analysis
## 1. Introduction

Frequency-domain analysis is a commonly used technique in signal processing. For example, in heart rate variation (HRV) analysis, high frequency (HF) and low frequency (LF) components are usually extracted from RR intervals based on power spectral analysis. As an industrial engineering, I am not familiar with this area, but I need this tool in my research. Although a lot of open-source software/libraries are available, I think it still necessary to understand what is going on. Therefore, I spent some time in frequency-domain analysis and make a little summary here.

## 2. Fourier Transform

Fourier transform is the basis of frequency-domain analysis. Fourier transform maps a signal from time domain to frequency domain. There are four kinds of Fourier transforms:

- Fourier series:
	$$ X(k)=\frac{1}{T} \int_0^T x(t) e^{-j2\pi k t/T} dt $$
- Discrete-time Fourier transform: 
	$$ X(f)=\sum_{n=-\infty}^{\infty} x(n) e^{-j2\pi fn} $$
- Continuous-time Fourier transform: 
	$$ X(f)=\int_{-\infty}^{\infty} x(t) e^{-j2\pi ft}dt $$
- Discrete Fourier transform: 
	$$ X(k)=\sum_{n=0}^{N-1} x(n) e^{-j2\pi kn/N} $$

Each kind of Fourier transform has its inverse Fourier transform, which reconstruct the original signal based on the frequency components.

Take Fourier series as an example. It can be shown that any periodic function $x(t)$ can be represented as linear combination of $\cos(2\pi m t/T)$ and $\sin(2\pi n t/T)$, where $T$ is the period of $x(t)$, i.e. $x(t+T)=x(t)$ for any $t$, and $m,n$ are integers,
$$
	x(t)=\sum_{m=0}^{\infty} a_m \cos(2\pi mt/T) + \sum_{n=1}^{\infty} b_n \sin(2\pi nt/T)
$$
Here $a_m$ and $b_n$ are real numbers. According to Euler's formula
$$
	e^{jx}=\cos x + j \sin x
$$
where $j=\sqrt{-1}$, then we get
$$
	x(t)=\sum_{k=-\infty}^{\infty} X(k) e^{j2\pi k t/T}
$$
Now $x(t)$ is represented by a linear combination of $e^{j2\pi k t/T}$, and $X(k)$ are the coefficients (complex numbers), $k$ is integer. Recall the Euler's formula, $e^{j2\pi k t/T}$ has period $T/k$ or frequency $k/T$. Thus $x(t)$ has been discomposited into combination of components in different frequency! It can be shown that the coefficient $X(k)$ can be calculated as
$$
	X(k)=\frac{1}{T} \int_0^T x(t) e^{-j2\pi k t/T} dt
$$
This equation is Fourier series. Therefore, Fourier series calculates the coefficients for each frequency component. And the previous equation is inverse Fourier series, which reconstruct the original time-domain signal based on the coefficients of frequency components.

Other kinds of Fourier transforms are similar. And the characteristics of each Fourier transform are listed in the following table.

<table>
<tr>
<th style="text-align:center;border:1px solid"> &nbsp; </th>
<th colspan="2" style="text-align:center;border:1px solid">Time domain</th>
<th colspan="2" style="text-align:center;border:1px solid">Frequency domain</th>
</tr>
<tr>
<td style="text-align:center;border:1px solid">Fourier series</td>
<td style="text-align:center;border:1px solid">continuous</td>
<td style="text-align:center;border:1px solid">periodic</td>
<td style="text-align:center;border:1px solid">discrete</td>
<td style="text-align:center;border:1px solid">nonperiodic</td>
</tr>
<tr>
<td style="text-align:center;border:1px solid">Discrete-time Fourier transform</td>
<td style="text-align:center;border:1px solid">discrete</td>
<td style="text-align:center;border:1px solid">nonperiodic</td>
<td style="text-align:center;border:1px solid">continuous</td>
<td style="text-align:center;border:1px solid">periodic</td>
</tr>
<tr>
<td style="text-align:center;border:1px solid">Continuous-time Fourier transform</td>
<td style="text-align:center;border:1px solid">continuous</td>
<td style="text-align:center;border:1px solid">nonperiodic</td>
<td style="text-align:center;border:1px solid">continuous</td>
<td style="text-align:center;border:1px solid">nonperiodic</td>
</tr>
<tr>
<td style="text-align:center;border:1px solid">Discrete Fourier transform</td>
<td style="text-align:center;border:1px solid">discrete</td>
<td style="text-align:center;border:1px solid">periodic</td>
<td style="text-align:center;border:1px solid">discrete</td>
<td style="text-align:center;border:1px solid">periodic</td>
</tr>
</table>

For example, Fourier series takes a continuous periodic function $x(t)$ as input, and produces discrete non-periodic coefficients $X(k)$ as output. The table shows something interesting. Discrete in one domain always related to periodic in the other domain, while continuous in one domain always related to nonperiodic in the other domain.

Since digital signals are stored in discrete values in computer, discrete-time Fourier transform (DTFT) and discrete Fourier transform (DFT) are more commonly used. DTFT takes discrete nonperiodic signal as input, and produces its frequency components as continuous periodic series. DFT takes discrete periodic signal as input, and produces its frequency components as a discrete periodic signal. If some conditions are satisfied, e.g. $N=2^n$, fast Fourier transform (FFT) algorithms can be employed to calculate DFT efficiently.

Fourier transform can be considered from the point of view [function basis and kernel method](../vector-to-function-basis-kernel/ "function basis and kernel method"), which is the concern of my another article.

There are other transforms such as z-transform and Laplace transform. They have very similar form with Fourier transform. For example, in z-transform
$$
	X(z)=\sum_{n=-\infty}^{\infty} x(n) z^{-n}
$$
Here $z$ is a complex number. Let
$$
	z=e^{jw}
$$
we get DTFT.

## 3. Energy Spectrum and Power Spectrum

Energy spectrum is defined as the energy on a specific frequency
$$
	S_x (f) = |X(f)|^2
$$
If we sum up or integrate the energy spectrum over $(-\infty, \infty)$ for Fourier series and continuous Fourier series, and over $[-1, 1]$ for DFT and DTFT, we can get the total energy.

However, sometimes a signal does not have Fourier transform (continuous Fourier transform or DTFT), i.e. $|X(f)|$ go to infinity, we define power spectrum as the $S_x (f)$ divided by the integral interval of Fourier transform.
$$
	R_x (f)=\lim_{T \rightarrow \infty} \frac{1}{2T} |\mathcal{F} (x)|^2
$$
Here $\mathcal{F} (x)$ applies Fourier transform to $x(t)$ over $[-T, T]$ at frequency $f$. It can be shown that $R_x (f)$ can be calculated as the Fourier transform of $r_x(\tau)$
$$
	R_x (f)=\mathcal{F} (r_x(\tau))
$$
where
$$
	r_x(\tau)=\lim_{T \rightarrow \infty} \frac{1}{2T} \int_{-T}^T x(t+\tau) x(t) dt
$$

## 4. Stationary Stochastic Process

Until now we are talking about deterministic signals, i.e. $x(t)$ is a specific function of $t$. Sometimes it is necessary to consider stochastic process, i.e. $x_t$ is a random variable. Here I use subscript to distinguish stochastic process from deterministic signal.

For a stochastic process $x_1, x_2, \cdots, x_N$, if the correlation between any two variables depends only on the difference in time
$$
	Cor(x_{n+k}, x_n)=Cor(x_{1+k},x_1)=r(k)
$$
for any $n$, and the expected value is certain
$$
	E(x_n)=\mu
$$
The stochastic process is weakly stationary or wide-sense stationary (WSS).

For WSS process, we cannot apply Fourier transform to the original series directly since they are random numbers. But we can apply Fourier transform to its autocorrelation function $r(k)$. Recall Section 3,
$$
	r_x(k)=\lim_{T \rightarrow \infty} \frac{1}{N} \sum_{0}^{N-1} x(t+k) x(t) dt
$$
is the autocorrelation for WSS process with zero mean and variance 1 (for other WSS process, it will be proportional). The resulting value
$$
	R_x (f)=\mathcal{F} (r_k)
$$
is the power spectrum.

## 5. Power Spectrum Estimation

Power spectrum is calculated as the Fourier transform of the autocorrelation function (ACF). However, in practice, the series is usually not infinite, and we rarely know the true ACF. Thus we need to estimate power spectrum. There are two categories of methods for power spectrum estimation: non-parametric method and parametric method.

In non-parametric method, ACF is estimated directly by data. Periodogram is a non-parametric estimation, where
$$
	\hat{r}(k)=\frac{1}{N} \sum_{n=0}^{N-1-k} x(n)x(n+k)
$$
here $N$ is the length of the signal. It is equivalent to the ACF of an infinite series $y(n)$ where $y(n)=x(n)$ when $n=0,1,\cdots,N-1$ and $y(n)=0$ for other $n$.

In parametric method, a model, e.g. autoregressive moving average (ARMA) model, is fitted at first. Then the ACF can be calculated.

## 6. Some Comments

The constraint in power spectrum analysis is that the process should be stationary. However, in a lot of situations, this assumption is not satisfied. Furthermore, power spectrum analysis need a long series of data to obtain good estimation. Therefore, maybe state-space model is a better choice in a number of situations.

## For more information

About ARMA model:

- Cryer, J. D., & Chan, K. S. (2008). *Time series analysis: with applications in R*. Springer.

About Fourier transform:

- [http://www.thefouriertransform.com/](http://www.thefouriertransform.com/)

About power spectrum:

- Hayes, M. H. (2009). *Statistical digital signal processing and modeling*. John Wiley & Sons.