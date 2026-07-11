# Spring Secirty

## ログイン前にリンクを踏んだところに、ログインしたら遷移させる

`SavedRequestAwareAuthenticationSuccessHandler` が前のリクエストを保持する物らしいので、これを使ってSuccessHandlerを生成する。セットする属性はコードを参照。

```jsx
package jp.co.isid.advtraining.security;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.access.AccessDeniedException;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.authentication.configuration.GlobalAuthenticationConfigurerAdapter;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.AuthenticationEntryPoint;
import org.springframework.security.web.access.AccessDeniedHandler;
import org.springframework.security.web.access.AccessDeniedHandlerImpl;
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
import org.springframework.security.web.authentication.SavedRequestAwareAuthenticationSuccessHandler;
import org.springframework.security.web.csrf.MissingCsrfTokenException;
import org.springframework.security.web.util.matcher.AntPathRequestMatcher;

import jp.co.isid.advtraining.service.LoginService;

@Configuration
@EnableWebSecurity // SpringSecurity4

public class WebSecurityConfiguration extends WebSecurityConfigurerAdapter {
	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http.authorizeRequests().antMatchers("/","/images/**", "/css/**", "/js/**","/login","/login?**").permitAll() // 指定パスへは全ユーザーアクセス許可
		.anyRequest().authenticated(); // その他のリクエストは認証が必要

		http.formLogin()
//				.defaultSuccessUrl("/adv21/menu", true) // ログイン成功時の遷移先
				.successHandler(successHandler())
				.usernameParameter("esqId") // ログインフォームで使用するユーザー名のinput name
				.passwordParameter("password")// ログインフォームで使用するパスワードのinput name
				.and().logout()
				// ログアウトがパス(GET)の場合設定する（CSRF対応）
				.logoutRequestMatcher(new AntPathRequestMatcher("/adv21/logout**"))
				// .logoutUrl("/logout") // ログアウトがPOSTの場合設定する
				.permitAll();
		http.exceptionHandling()
        // 通常のRequestとAjaxを両方対応するSessionTimeout用
        .authenticationEntryPoint(authenticationEntryPoint())
        // csrfはsessionがないと動かない。SessionTimeout時にPOSTすると403 Forbiddenを必ず返してしまうため、
        // MissingCsrfTokenExceptionの時はリダイレクトを、それ以外の時は通常の扱いとする。
        .accessDeniedHandler(accessDeniedHandler());
	}

	@Bean
	AuthenticationSuccessHandler successHandler() {
	    SavedRequestAwareAuthenticationSuccessHandler handler = new SavedRequestAwareAuthenticationSuccessHandler();
	    handler.setTargetUrlParameter("redirectTo");
	    handler.setDefaultTargetUrl("/adv21/menu");
	    return handler;
	}

	@Bean
	AccessDeniedHandler accessDeniedHandler() {
		return new AccessDeniedHandler() {
			@Override
			public void handle(HttpServletRequest request, HttpServletResponse response,
					AccessDeniedException accessDeniedException) throws IOException, ServletException {
				if (accessDeniedException instanceof MissingCsrfTokenException) {
					authenticationEntryPoint().commence(request, response, null);
				} else {
					new AccessDeniedHandlerImpl().handle(request, response, accessDeniedException);
				}
			}
		};
	}

	@Bean
	AuthenticationEntryPoint authenticationEntryPoint() {
		System.out.println("AuthenticationEntryPoint通過");
		return new TimeoutUrlAuthenticationEntryPoint("/login");
	}

	@Configuration
	protected static class AuthenticationConfiguration extends GlobalAuthenticationConfigurerAdapter {

		@Autowired
		private LoginService loginService;

		@Override
		public void init(AuthenticationManagerBuilder auth) throws Exception {
			// 認証するユーザーを設定する
			auth.userDetailsService(loginService)
					// 入力値をbcryptでハッシュ化した値でパスワード認証を行う
					.passwordEncoder(new BCryptPasswordEncoder());
		}
	}
}
```