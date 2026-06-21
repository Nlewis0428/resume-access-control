<?php
/**
 * Illustrative pattern only — shows the general approach to gating a file
 * behind a validated request rather than serving it from a public URL.
 * Not the live implementation.
 */

// The resume PDF lives outside the web root, so there's no direct public URL to it.
define( 'RESUME_FILE_PATH', '/path/outside/webroot/resume.pdf' );

/**
 * Runs after a Contact Form 7 submission is validated and logged by Flamingo.
 * Generates a one-time, time-limited token instead of exposing the file directly.
 */
function generate_resume_access_token( $submission_email ) {
    $token = bin2hex( random_bytes( 16 ) );
    $expires = time() + ( 60 * 60 * 24 ); // 24-hour window

    // Store token, associated email, and expiry — e.g. in a custom table or transient.
    set_transient( "resume_token_{$token}", [
        'email'   => $submission_email,
        'expires' => $expires,
    ], DAY_IN_SECONDS );

    return $token;
}

/**
 * Validates an incoming token and serves the file only if it matches and hasn't expired.
 */
function serve_resume_if_valid( $token ) {
    $record = get_transient( "resume_token_{$token}" );

    if ( ! $record || $record['expires'] < time() ) {
        wp_die( 'This link is invalid or has expired.', 403 );
    }

    delete_transient( "resume_token_{$token}" ); // single use

    header( 'Content-Type: application/pdf' );
    header( 'Content-Disposition: inline; filename="resume.pdf"' );
    readfile( RESUME_FILE_PATH );
    exit;
}
