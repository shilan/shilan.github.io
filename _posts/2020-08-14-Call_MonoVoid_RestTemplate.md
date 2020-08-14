I had a rest service that was implemented with WebFlux. Was a POST that returned Mono<Void>.
The client however needed to call it with RestTemplate.

	/**
	 * Indicates whether the response has a message body.
	 * <p>Implementation returns {@code false} for:
	 * <ul>
	 * <li>a response status of {@code 1XX}, {@code 204} or {@code 304}</li>
	 * <li>a {@code Content-Length} header of {@code 0}</li>
	 * </ul>
	 * @return {@code true} if the response has a message body, {@code false} otherwise
	 * @throws IOException in case of I/O errors
	 */
	public boolean hasMessageBody() throws IOException {
		HttpStatus status = HttpStatus.resolve(getRawStatusCode());
		if (status != null && (status.is1xxInformational() || status == HttpStatus.NO_CONTENT ||
				status == HttpStatus.NOT_MODIFIED)) {
			return false;
		}
		if (getHeaders().getContentLength() == 0) {
			return false;
		}
		return true;
	}

