package cedge.epa.portal.util;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import io.jsonwebtoken.security.Keys;

import javax.crypto.SecretKey;
import java.util.*;

public class JwtUtil {

    private static final SecretKey SECRET_KEY = Keys.secretKeyFor(SignatureAlgorithm.HS256);
    private static final Set<String> INVALIDATED_TOKENS = new HashSet<>();

    public static String generateToken(String username) {
        String token = Jwts.builder()
                .setSubject(username)
                .setExpiration(new Date(System.currentTimeMillis() + 60000)) // Token expiration time (1 hour in this example)
                .signWith(SECRET_KEY)
                .compact();

        System.out.println("Generated Token: " + token);
        return token;
    }

    public static boolean validateToken(String token) {
        try {
            if (INVALIDATED_TOKENS.contains(token)) {
                System.out.println("Token is invalidated");
                return false; // Token is invalidated
            }

            Claims claims = Jwts.parserBuilder()
                    .setSigningKey(SECRET_KEY)
                    .build()
                    .parseClaimsJws(token)
                    .getBody();

            // Check if the token has expired
            Date expirationDate = claims.getExpiration();
            if (expirationDate != null && expirationDate.before(new Date())) {
                System.out.println("Token is expired");
                return false; // Token is expired
            }

            // You can add more checks based on your requirements

            return true; // Token is valid
        } catch (Exception e) {
            System.out.println("Token validation failed");
            e.printStackTrace();
            return false; // Token validation failed
        }
    }

    // Client-side logout (add token to the list of invalidated tokens)
    public static void logout(String token) {
        INVALIDATED_TOKENS.add(token);
        System.out.println("Logout successful");
    }
}
