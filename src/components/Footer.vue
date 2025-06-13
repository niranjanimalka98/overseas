<template>
  <section>
    <footer class="amadeus-footer">
      <div class="footer-container">
        <!-- Main footer content -->
        <div class="footer-main">
          <div class="footer-left">
            <h2 class="company-name">Amadeus IT Group</h2>
            <div class="company-address">
              <p>C. Salvador de Madariaga, 1</p>
              <p>28027 Madrid</p>
              <p>Spain</p>
            </div>
          </div>

          <div class="footer-right">
            <div class="social-section">
              <span class="follow-text">Follow us on</span>
              <div class="social-icons">
                <a
                  href="https://twitter.com/amadeus"
                  target="_blank"
                  rel="noopener noreferrer"
                  aria-label="Follow us on Twitter"
                >
                  <img src="../assets/Icons/Twitter White.svg" class="social-icon" />
                </a>
                <a
                  href="https://youtube.com/amadeus"
                  target="_blank"
                  rel="noopener noreferrer"
                  aria-label="Follow us on YouTube"
                >
                  <img src="../assets/Icons/Youtube White.svg" />
                </a>
              </div>
            </div>
          </div>
        </div>
        <!-- Footer bottom -->
        <div class="footer-bottom">
          <div class="footer-divider"></div>

          <div class="footer-bottom-content">
            <div class="copyright">
              <p>Copyright © 2022 Amadeus Hotels. All rights reserved.</p>
            </div>

            <div class="photo-credits">
              <p>
                Photos by
                <a
                  href="https://unsplash.com/@felixmooneeram"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="credit-link"
                >
                  Felix Mooneeram
                </a>
                &
                <a
                  href="https://unsplash.com/@serge_kutuzov"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="credit-link"
                >
                  Serge Kutuzov
                </a>
                on
                <a
                  href="https://unsplash.com"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="credit-link"
                >
                  Unsplash
                </a>
              </p>
            </div>
          </div>
        </div>
      </div>
    </footer>
  </section>
</template>

<script setup></script>

<style scoped>
.amadeus-footer {
  background-color: #1a1a1a;
  color: #ffffff;
  padding: 2rem 0 1rem;
  font-family:
    -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans',
    'Helvetica Neue', sans-serif;
}

.footer-container {
  
}

.footer-main {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 2rem;
  padding-left: 3rem;
  padding-right: 3rem;
}

.footer-left {
  flex: 1;
}

.company-name {
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 1rem;
  color: #ffffff;
}

.company-address {
  line-height: 1.6;
}

.company-address p {
  margin: 0;
  color: #cccccc;
  font-size: 1rem;
}

.footer-right {
  display: flex;
  align-items: center;
}

.social-section {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.follow-text {
  font-size: 1rem;
  color: #cccccc;
  white-space: nowrap;
}

.social-icons {
  display: flex;
  gap: 0.75rem;
}

.social-link {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  background-color: #ffffff;
  border-radius: 4px;
  transition: all 0.3s ease;
  text-decoration: none;
}

.social-link:hover {
  background-color: #f0f0f0;
  transform: translateY(-2px);
}

.social-icon {
  width: 20px;
  height: 20px;
  color: #1a1a1a;
}

.footer-bottom {
  margin-top: 2rem;
}

.footer-divider {
  height: 1px;
  background-color: #333333;
  margin-bottom: 1.5rem;
}

.footer-bottom-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 1rem;
  padding-left: 3rem;
  padding-right: 3rem;
}

.copyright p {
  margin: 0;
  color: #888888;
  font-size: 0.9rem;
}

.photo-credits p {
  margin: 0;
  color: #888888;
  font-size: 0.9rem;
}

.credit-link {
  color: #888888;
  text-decoration: underline;
  transition: color 0.3s ease;
}

.credit-link:hover {
  color: #cccccc;
}

/* Tablet styles */
@media (max-width: 992px) {
  .footer-main {
    flex-direction: column;
    align-items: center;
    text-align: center;
    gap: 2rem;
  }

  .footer-left {
    flex: none;
  }

  .footer-right {
    justify-content: center;
  }

  .footer-bottom-content {
    flex-direction: column;
    text-align: center;
    gap: 1rem;
  }
}

/* Mobile styles */
@media (max-width: 768px) {
  .amadeus-footer {
    padding: 1.5rem 0 1rem;
  }

  .footer-container {
    padding: 0 1rem;
  }

  .footer-main {
    margin-bottom: 1.5rem;
    gap: 1.5rem;
  }

  .company-name {
    font-size: 1.25rem;
    margin-bottom: 0.75rem;
  }

  .company-address p {
    font-size: 0.9rem;
  }

  .social-section {
    flex-direction: column;
    gap: 0.75rem;
    align-items: center;
  }

  .follow-text {
    font-size: 0.9rem;
  }

  .social-icons {
    gap: 1rem;
  }

  .footer-divider {
    margin-bottom: 1rem;
  }

  .copyright p,
  .photo-credits p {
    font-size: 0.8rem;
    line-height: 1.4;
  }
}

/* Small mobile styles */
@media (max-width: 480px) {
  .footer-container {
    padding: 0 0.75rem;
  }

  .company-name {
    font-size: 1.1rem;
  }

  .company-address p {
    font-size: 0.85rem;
  }

  .social-link {
    width: 36px;
    height: 36px;
  }

  .social-icon {
    width: 18px;
    height: 18px;
  }

  .copyright p,
  .photo-credits p {
    font-size: 0.75rem;
  }
}
</style>
