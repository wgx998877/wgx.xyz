title : numpy.trapz简介
---
#numpy.trapz
来自scipy.org
##源代码source code
	def trapz(y, x=None, dx=1.0, axis=-1):
	"""
	Integrate along the given axis using the composite trapezoidal rule.
	Integrate `y` (`x`) along given axis.
	Parameters
	----------
	y : array_like
	Input array to integrate.
	x : array_like, optional
	If `x` is None, then spacing between all `y` elements is `dx`.
	dx : scalar, optional
	If `x` is None, spacing given by `dx` is assumed. Default is 1.
	axis : int, optional
	Specify the axis.
	Returns
	-------
	trapz : float
	Definite integral as approximated by trapezoidal rule.
	See Also
	--------
	sum, cumsum
	Notes
	-----
	Image [2]_ illustrates trapezoidal rule -- y-axis locations of points
	will be taken from `y` array, by default x-axis distances between
	points will be 1.0, alternatively they can be provided with `x` array
	or with `dx` scalar. Return value will be equal to combined area under
	the red lines.
	References
	----------
	.. [1] Wikipedia page: http://en.wikipedia.org/wiki/Trapezoidal_rule
	.. [2] Illustration image:
	http://en.wikipedia.org/wiki/File:Composite_trapezoidal_rule_illustration.png
	Examples
	--------
	>>> np.trapz([1,2,3])
	4.0
	>>> np.trapz([1,2,3], x=[4,6,8])
	8.0
	>>> np.trapz([1,2,3], dx=2)
	8.0
	>>> a = np.arange(6).reshape(2, 3)
	>>> a
	array([[0, 1, 2],
	[3, 4, 5]])
	>>> np.trapz(a, axis=0)
	array([ 1.5, 2.5, 3.5])
	>>> np.trapz(a, axis=1)
	array([ 2., 8.])
	"""
	y = asanyarray(y)
	if x is None:
	d = dx
	else:
	x = asanyarray(x)
	if x.ndim == 1:
	d = diff(x)
	# reshape to correct shape
	shape = [1]*y.ndim
	shape[axis] = d.shape[0]
	d = d.reshape(shape)
	else:
	d = diff(x, axis=axis)
	nd = len(y.shape)
	slice1 = [slice(None)]*nd
	slice2 = [slice(None)]*nd
	slice1[axis] = slice(1, None)
	slice2[axis] = slice(None, -1)
	try:
	ret = (d * (y[slice1] +y [slice2]) / 2.0).sum(axis)
	except ValueError: # Operations didn't work, cast to ndarray
	d = np.asarray(d)
	y = np.asarray(y)
	ret = add.reduce(d * (y[slice1]+y[slice2])/2.0, axis)
	return ret
